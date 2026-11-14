# Viabilidade: esta máquina consegue fazer os 14 projetos de graça?

**Resposta curta: 12 dos 14 sem ressalva, 2 com ressalva real, custo R$ 0.**
Duas linguagens da lista original não têm caminho gratuito e estão marcadas
para exclusão justificada.

Documento verificado por inspeção da máquina em 2026-11-13, não por estimativa.

## A máquina

| | |
|---|---|
| CPU | Intel Core i5-8250U, 4 núcleos / 8 threads, 1,6 GHz base (classe U, com throttling) |
| RAM | **7,9 GB** |
| GPU 1 | Intel UHD Graphics 620 (1 GB compartilhado) |
| GPU 2 | **NVIDIA GeForce MX150, 2 GB, compute capability 6.1**, driver 582.53 |
| Disco | 893 GB, **610 GB livres** |
| SO | Windows 11 Home Single Language + WSL2 (Ubuntu 26.04, kernel 6.18) |
| Toolchain já instalado | gcc 15.2, rustc 1.93, cmake, MSVC 19.5, Node, Python, Java (JRE), Perl, perf |

Dois fatos verificados que mudam o plano original para melhor:

1. **Existe GPU NVIDIA.** O `nvidia-smi` responde **dentro do WSL2** e
   `/dev/dxg` está presente, então CUDA roda localmente — não precisa de nuvem.
2. **O `perf` funciona no WSL2** com contadores de hardware reais
   (`perf_event_paranoid = 2`, PMU exposta). Medição de ciclos, instruções e
   erros de salto é possível sem Linux em metal puro. Já foi usado no P01.

## Veredito por projeto

| # | Projeto | Veredito | Observação |
|---|---|---|---|
| P01 | metal-vm | ✅ **feito** | C e Rust completos, CI 9/9, `perf` usado |
| P02 | forge-lang | ✅ viável | ~25 GB de disco para GHC, Coq, Isabelle, Lean, .NET. Isabelle quer 4–8 GB de RAM: rodar sozinho |
| P03 | otp-mesh | ⚠️ **com ressalva** | "milhões de conexões" não cabe em 8 GB. Ver [Ajustes](#ajustes-necessários-no-plano) |
| P04 | webstack | ✅ viável | Node, PHP, Ruby, Elm, PureScript, Dart — todos leves |
| P05 | datalab | ⚠️ **com ressalva** | SAS, MATLAB e Wolfram têm caminho gratuito; **Stata não tem** |
| P06 | gpu-arena | ✅ viável | CUDA local na MX150. **Triton exige sm_70**, a MX150 é sm_61 → Colab |
| P07 | engine | ⚠️ risco real | C++/Godot/Lua tranquilos. **Unreal 5 em 8 GB + MX150 2 GB é inviável** |
| P08 | tri-mobile | ⚠️ **com ressalva** | Android local ok; **iOS exige macOS** → só via CI |
| P09 | silicon | ✅ viável | Verilator, GHDL, Yosys, toolchain RISC-V: tudo livre e só CPU. Placa FPGA é opcional |
| P10 | query-lab | ✅ viável | Postgres, Oracle XE, SQL Server Developer, Neo4j, Fuseki — todos gratuitos, **um por vez** |
| P11 | chain | ✅ viável | Foundry, Hardhat, Aptos/Sui, Starknet, Solana: tudo local e gratuito |
| P12 | toolbelt | ⚠️ ajuste | Tudo gratuito **exceto VBA**: não há Office nesta máquina |
| P13 | legacy-bridge | ✅ viável | Caminho gratuito para **todos**, incluindo RPG e ABAP. Ver tabela abaixo |
| P14 | first-code | ✅ viável | Scratch, Blockly, Logo (FMSLogo), Smalltalk (Pharo), Pascal: todos livres |

## As quatro restrições que realmente limitam

### 1. RAM de 8 GB — a restrição transversal

É o gargalo real, não a CPU nem o disco. Afeta:

| Projeto | O que não cabe | Mitigação |
|---|---|---|
| P03 | k8s local + cluster BEAM + Kafka ao mesmo tempo | `kind` com 1 nó, Redpanda em vez de Kafka, e **escalar a meta para ~50 k conexões** |
| P05 | Spark em cluster | Spark em modo local com 2 executores |
| P10 | Oracle XE + SQL Server + Neo4j simultâneos | Um contêiner por vez, via `docker compose --profile` |
| P02 | Isabelle junto com qualquer outra coisa | Rodar isolado |
| P07 | Unreal 5 | Ver restrição 3 |

Mitigação geral: limitar a RAM do WSL2 em `%USERPROFILE%\.wslconfig` (ex.: 5 GB)
para o Windows não ser sufocado, e **nunca rodar dois stacks pesados juntos**.

### 2. macOS — bloqueio de hardware, com saída gratuita

Swift/iOS, Objective-C e Metal Shading Language exigem macOS. Não há emulação
legal nem gratuita.

**Saída:** runners `macos-latest` do GitHub Actions, que são **gratuitos e sem
limite de minutos em repositório público**. Dá para compilar e rodar os testes
de unidade do app iOS a cada push.

O que isso **não** resolve: desenvolvimento interativo (SwiftUI preview,
depuração no simulador, Instruments). O app iOS do P08 vai ser escrito "no
escuro", guiado por CI. É honesto declarar isso no README do projeto em vez de
fingir paridade com o Android.

Sobre Metal no P06: **compilar** o shader no runner é certo; **executar** kernel
de computação em runner Apple Silicon virtualizado precisa de teste — não
prometo antes de tentar.

### 3. Unreal Engine — a restrição mais dura

UE5 pede 16 GB de RAM e GPU com 4+ GB. Esta máquina tem 8 GB e 2 GB de VRAM. O
editor provavelmente abre e é inutilizável.

Três saídas, em ordem de preferência:

1. **UE 4.27 com projeto vazio**, só para fazer um grafo de Blueprints
   funcionar e gravar um GIF. Requisito bem menor; ainda assim aperta.
2. **Substituir a demonstração de programação visual**: o P14 já tem Blockly e
   Scratch, e dá para implementar um *editor de grafos de execução* próprio no
   P07 — o que demonstra mais entendimento do que arrastar nós no editor alheio.
3. Documentar como restrição de hardware e seguir. Blueprints é a única
   linguagem da lista que ficaria descoberta.

Recomendação: tentar (1); se o editor não abrir, fazer (2) e registrar o porquê.

### 4. Triton — exige GPU mais nova

Triton requer compute capability ≥ 7.0 (Volta). A MX150 é **6.1** (Pascal).

**Saída:** Google Colab, camada gratuita, dá uma T4 (sm_75). Serve para o kernel
em Triton do P06 e para qualquer comparação que precise de tensor cores. Os
outros seis alvos de GPU (CUDA, OpenCL, WGSL, GLSL, HLSL, Metal) não dependem
disso.

## Acesso gratuito às linguagens proprietárias

Esta é a tabela que decide o P05 e o P13:

| Linguagem | Caminho gratuito | Tipo |
|---|---|---|
| MATLAB | **Octave** local + MATLAB Online Basic (20 h/mês) | ✅ permanente |
| Wolfram | **Wolfram Engine** free for developers | ✅ permanente |
| SAS | **SAS OnDemand for Academics** (nuvem, cadastro) | ✅ permanente |
| **Stata** | **não existe** — sem edição gratuita, sem comunidade, sem trial aberto | ❌ |
| ABAP | **SAP BTP Trial**, ambiente ABAP no navegador | ⏳ 90 dias |
| RPG | **PUB400.com** — sistema IBM i público e gratuito para hobbistas, com compiladores RPG | ✅ permanente |
| PL/I | **Iron Spring PL/I**, compilador gratuito para Linux | ✅ permanente (subset) |
| Apex | **Salesforce Developer Edition**, org gratuita para sempre | ✅ permanente |
| Delphi | **Free Pascal + Lazarus**, ou Delphi Community Edition | ✅ permanente |
| COBOL | **GnuCOBOL** | ✅ permanente |
| VB.NET / C# | **.NET SDK** | ✅ permanente |
| **VBA** | Office não instalado. LibreOffice Basic é **outro dialeto**, não VBA | ⚠️ |
| Mojo | **Modular SDK**, gratuito para desenvolvimento (Linux/WSL2) | ✅ permanente |

Contas gratuitas a criar, no total: **4** — SAS OnDemand, SAP BTP Trial,
Salesforce Developer, Google Colab. Nenhuma pede cartão.

## Ajustes necessários no plano

Quatro mudanças concretas no ROADMAP, todas por restrição real e não por
preguiça:

### P03 — trocar a meta de escala

"Milhões de conexões simuladas" é fisicamente impossível em 8 GB. A meta passa
a ser **~50 mil conexões num nó**, com o número real medido e publicado, mais a
aritmética de quanto seria preciso para escalar. Demonstra a mesma competência:
a técnica de back-pressure e supervisão não muda com a escala — e um número
medido vale mais que um número aspiracional.

### P05 — excluir Stata, com justificativa escrita

É a única linguagem da lista sem **nenhum** caminho gratuito. Duas opções
honestas:

- **Excluir** e registrar no README: "sem edição gratuita; a função dela no
  projeto é coberta por R, que é livre e mais usado em publicação". É a
  recomendada.
- Escrever o `.do` equivalente e rotular **explicitamente** como "código
  revisado, não executado". Aceitável só se o rótulo estiver visível; código
  não executado apresentado como funcional é o tipo de coisa que destrói
  credibilidade quando descoberto.

### P12 — substituir VBA por AutoHotkey + LibreOffice Basic

Sem Office, não há host para VBA. O que o VBA demonstrava — automação de
aplicativo de escritório — fica coberto por **LibreOffice Basic**, rotulado como
tal e **não** como VBA, já que a sintaxe e o modelo de objetos diferem. O
AutoHotkey, que já estava no plano, cobre a automação de desktop e é nativo do
Windows.

Alternativa, se você quiser VBA de verdade: o trial de 1 mês do Microsoft 365
basta para escrever e gravar o módulo.

### P07 — decidir Unreal depois de tentar

Ver restrição 3. Não comprometa o README do P07 com Blueprints antes de o
editor abrir nesta máquina.

## O que esta máquina faz bem

Para não deixar só a lista de limites:

- **Todo o trabalho de CPU e de compilador é confortável.** P01, P02, P09, P10,
  P11, P13 e P14 — sete projetos, incluindo os dois tecnicamente mais
  impressionantes (o compilador com prova formal e a CPU em Verilog) — não
  pedem nada que falte aqui.
- **610 GB livres** absorvem GHC, Coq, Isabelle, .NET, JDK, Android SDK e
  contêineres sem aperto.
- **CUDA local** com `perf` funcionando no WSL2 é uma combinação melhor do que
  eu esperava: dá para medir kernels de GPU e contadores de CPU na mesma
  máquina.
- **Repositório público = Actions gratuito e ilimitado** nos runners padrão,
  incluindo macOS e Windows. É o que compra o iOS, o MSVC e a matriz de
  compiladores sem custo.

## Conclusão

**Custo em dinheiro: R$ 0.** Nenhum projeto exige compra de software ou de
hardware.

O preço real é em **tempo e em escopo**: 8 GB de RAM forçam rodar um stack por
vez, a ausência de Mac torna o app iOS guiado por CI em vez de interativo, e
Unreal provavelmente fica de fora. Duas linguagens da lista original
(Stata e VBA) perdem o caminho gratuito e devem sair com justificativa escrita —
o que, num portfólio que se propõe honesto sobre cobertura, é conteúdo e não
falha.
