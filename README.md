# poliglota — índice do portfólio

Catorze projetos de engenharia que, somados, cobrem ~130 linguagens de programação. Cada
linguagem aparece onde ela é a escolha tecnicamente certa — com benchmark, trade-off escrito e
CI que compila tudo.

> **Status:** em construção. A tabela abaixo é a fonte da verdade do progresso.

## O que este portfólio demonstra (e o que não demonstra)

**Demonstra:** profundidade real em ~12 linguagens-núcleo (C, C++, Rust, Python, TypeScript,
Haskell, Elixir, Go, C#, SQL, Verilog, Assembly) e fluência de leitura, porte e avaliação de
trade-off nas outras ~120.

**Não demonstra:** domínio absoluto de 130 linguagens. Isso não existe — nem entre os autores
delas. Cada README declara exatamente qual é o nível de uso de cada linguagem no projeto:
`núcleo` (implementação completa), `porte` (núcleo reimplementado para comparação) ou
`referência` (módulo equivalente documentado, geralmente por restrição de licença/hardware).

Essa distinção é deliberada. Um portfólio que mente sobre profundidade é descoberto na primeira
pergunta técnica; um que calibra honestamente ganha credibilidade no resto.

## Os projetos

| # | Repositório | Tema | Linguagens | Status |
|---|---|---|---|---|
| P01 | **[metal-vm](https://github.com/Brunowdgaster/metal-vm)** | VM de bytecode em implementacoes comparadas sob medicao | C, C++, Rust, Zig, Assembly ×3, Nim, Odin, D, V, Ada | 🟨 **7 de 12: C e Rust completos, mais 5 portes** (Nim, Odin, D, V, Ada) em 9/9 contra o mesmo `.fbc`. Tabela comparativa de linhas × build × execucao × RSS; o Nim fez em **255 linhas** e o **D tem GC e ficou agrupado com os que nao tem**. 54/54 na compatibilidade cruzada, CI 10/10. C++/Zig/Assembly pendentes |
| P02 | **[forge-lang](https://github.com/Brunowdgaster/forge-lang)** | Linguagem funcional com inferencia total + prova formal | Haskell, OCaml, F#, Racket, Scheme, CL, Clojure, Prolog, Lean, Coq, Agda, Idris, Isabelle, TLA+, WAT | 🟨 **Compilador completo, com closures.** Hindley-Milner total em Haskell, ordem superior e aplicacao parcial, 39/39 testes nas 3 VMs do P01. Portes e provas pendentes |
| P03 | **[otp-mesh](https://github.com/Brunowdgaster/otp-mesh)** | Plataforma distribuida de tempo real, chaos-tested | Elixir, Erlang, Gleam, Go, Rust, Scala, Clojure, HCL, YAML | ✅ **Completo, 8/8 linguagens, CI 9/9.** 700 mil conexoes supervisionadas a 3.534 B cada; protocolo verificado byte a byte em 4 linguagens; travessia de NIF em ~30 ns; Loom custa 1.323 B mas sofre inanicao total onde a BEAM degrada 21x; `agent` de Clojure fica na mesma ordem de grandeza do GenServer; 20 asseroes de IaC. **Auditoria completa das medicoes em docs/AUDITORIA.md: 5 corrigidas.** Ambiente 100% livre |
| P04 | `webstack` | Produto web com 3 front-ends comparados | TypeScript, JS, HTML, CSS, Sass, Elm, PureScript, PHP, Ruby, Dart, GraphQL | ⬜ scaffold |
| P05 | `datalab` | Mesmo modelo estatístico em 5 linguagens | Python, R, Julia, Scala, Fortran, Mojo, Octave, SAS, Stata, Wolfram | ⬜ scaffold |
| P06 | `gpu-arena` | Mesmo kernel em 7 APIs de GPU | CUDA, OpenCL, Triton, WGSL, GLSL, HLSL, Metal | ⬜ scaffold |
| P07 | `engine` | Motor próprio vs. Godot vs. Unreal | C++, C#, Lua, GDScript, Blueprints, HLSL, GLSL | ⬜ scaffold |
| P08 | `tri-mobile` | Mesmo app: Android nativo, iOS nativo, Flutter | Kotlin, Java, Swift, Objective-C, Dart | ⬜ scaffold |
| P09 | `silicon` | CPU RISC-V em Verilog rodando o VM do P01 | Verilog, SystemVerilog, VHDL, Assembly, C, Rust, MicroPython, Tcl | ⬜ scaffold |
| P10 | `query-lab` | Mesmo domínio em relacional, grafo e RDF | SQL, PL/pgSQL, PL/SQL, T-SQL, Cypher, SPARQL, GraphQL | ⬜ scaffold |
| P11 | `chain` | Mesmo contrato em Solidity e Vyper, fuzzing diferencial | Solidity, Vyper, Move, Cairo, Rust | ⬜ scaffold |
| P12 | `toolbelt` | Automação de uso diário, cada peça na linguagem natural | Bash, PowerShell, Perl, AWK, Lua, Tcl, Raku, Groovy, AHK, VBA, Nix | ⬜ scaffold |
| P13 | `legacy-bridge` | COBOL → C# com equivalência provada por testes | COBOL, Delphi, VB.NET, C#, PL/I, RPG, ABAP, Apex | ⬜ scaffold |
| P14 | `first-code` | Currículo de programação visual | Scratch, Blockly, Logo, Smalltalk, Pascal | ⬜ scaffold |

Legenda: ⬜ pasta criada · 🟨 em desenvolvimento · ✅ completo (README + CI verde + resultado medido)

Apenas o P01 está publicado. Os demais aparecem como `nome` em texto simples até
existirem — link para repositório inexistente é pior que link ausente.

## O que os projetos publicados já estabeleceram

Para que este índice não seja só uma lista de intenções, o estado do único
projeto publicado:

- **O custo da segurança do Rust num interpretador: +66,7 % em instruções x86
  por instrução de bytecode** e +78,7 % em saltos, contra o mesmo algoritmo em
  C. Medido com `perf`, reproduzido em duas CPUs.
- **O folclore do "computed goto é mais rápido" não se sustenta**: o efeito
  variou de −10,3 % a +11,8 % em três CPUs, e a taxa de erro de salto (0,05 %)
  refuta a explicação usual. O ganho real, onde existe, vem de executar dois
  saltos a menos por despacho.
- **Duas implementações independentes com bytecode byte a byte idêntico**,
  verificado em 42 checagens a cada push.

### P02 — forge-lang

- **A promessa do P01 foi testada e custou uma instrucao.** O P01 afirmou que
  o bytecode foi "desenhado para o P02 estender". Quando o forge precisou de
  closures, a pergunta virou mensuravel: quanto da VM precisa mudar? Resposta:
  **um opcode** (`CALLI`, chamada indireta) e ~45 linhas somando as duas
  implementacoes. Closure e array comum, entao o heap e o coletor existentes
  bastaram. O CI do P02 clona e compila o P01 a cada push para manter isso
  verificado.

### P03 — otp-mesh

- **700 mil processos supervisionados em 2,3 GB**, num notebook cujo WSL2 vê
  3,8 GB. Custo estável em 3.534 bytes por conexão de mil a 700 mil: sem
  degradação superlinear. A minha própria estimativa na revisão de viabilidade
  era "~50 mil" — **14 vezes abaixo** do medido.
- **A árvore de supervisão aguenta falha em massa.** Matando 210 mil conexões
  de uma vez, o supervisor é o mesmo processo depois, e o processo
  `:permanent` de estatísticas volta em milissegundos com a contagem correta, relida
  do registro.
- **Um socket real custa 1,9x um processo BEAM: 7,1 KB contra 3,8 KB.** O
  gateway WebSocket em Go, escrito sem dependências para que os buffers fossem
  escolha medida e não herdada — e escolher 1 KiB em vez dos 4 KiB que as
  bibliotecas usam por padrão economiza 35% por conexão. O folclore diz
  "milhares contra milhões"; o que separa as plataformas em escala não é o
  custo por conexão, é supervisão e isolamento de falha.
- **Ambiente com zero recursos pagos**, 14 ferramentas, 11/11 no teste de
  fumaça — incluindo um cluster Kubernetes real. Quatro escolhas para evitar
  armadilha de licença: Docker Engine (não Desktop), OpenTofu (não
  Terraform/BUSL), NATS (não Kafka/Redpanda) e providers locais (nuvem pede
  cartão).

### E o que não deu certo

Três versões da conclusão principal do P01 foram publicadas e corrigidas
conforme novas máquinas apareceram, e o `git log` mostra as três. No P02, dois
testes nasceram errados e um bug de alocação de slot foi pego ao escrever o
comentário que explicava por que ele não existia.

E o portfólio encontrou **o mesmo modo de falha oito vezes** — instrumento que
esconde o próprio sinal: harness de benchmark,
injetor de caos, provisionador e verificador — todos redirecionavam a saída e
ignoravam o código de retorno. Conhecer um modo de falha não imuniza contra
ele; o que imuniza é a ferramenta reclamar alto. Isso também é parte do que o
portfólio demonstra.

## Por onde começar a ler

- Quer ver **engenharia de sistemas**: P01 → P09 (a VM roda na CPU que eu projetei).
- Quer ver **rigor teórico**: P02 (type checker provado correto em Lean).
- Quer ver **produto entregável**: P04 e P03.
- Quer ver **algo que quase ninguém faz**: P13 (COBOL → C# com equivalência provada).

## Ordem de construção

A ordem original abaixo foi ordenada por **dependência técnica e impressão**.
Depois de três projetos, ela se mostrou errada para o objetivo declarado —
cobrir o máximo de linguagens — porque o rendimento de linguagens por esforço
varia brutalmente entre projetos. A ordem revista está em
[CONTEXTO.md](CONTEXTO.md#4-a-ordem-de-trabalho-decidida): terminar o P03 e
então **P12 → P10 → P14 → P05 → P06**, que levaria de 5 para ~45 linguagens.

Ordem original, mantida para registro:

| Fase | Projetos | Racional |
|---|---|---|
| 1 | P01, P12 | O VM é a espinha do P02 e do P09; o toolbelt constrói o ambiente de todos |
| 2 | P02, P10 | Alto valor técnico, só precisa de CPU |
| 3 | P03, P04 | As peças legíveis em 10 segundos por qualquer revisor |
| 4 | P09, P06 | Hardware e GPU — tecnicamente as mais impressionantes |
| 5 | P05, P07, P11 | Domínios especializados |
| 6 | P08, P13, P14 | Dependem de macOS, licenças ou tempo — fecham a cobertura |

**Regra:** nenhum projeto é publicado antes de ter README completo, CI verde e um resultado
medido. Quatro repositórios excelentes valem mais que catorze abandonados.

## Retomando o trabalho

Se você está chegando agora — ou se a sessão de trabalho foi reiniciada —
**[CONTEXTO.md](CONTEXTO.md)** é o ponto de partida: objetivo, estado medido,
convenções já estabelecidas, armadilhas conhecidas desta máquina e a ordem de
trabalho decidida. Nada precisa ser redescoberto.

Resumo do estado em 2026-11-14: **5 linguagens com implementação profunda**
(Rust, Haskell, C, Elixir, Go) de ~130 planejadas, em 3 projetos publicados de
14. Mais 2 linguagens criadas do zero (`.fasm` e `forge`) e 4 auxiliares
genuinamente usadas.

## Viabilidade nesta máquina

Antes de prometer 14 projetos, a pergunta óbvia: o hardware aguenta, e de graça?
Resposta verificada por inspeção, não estimada, em
**[VIABILIDADE.md](VIABILIDADE.md)**:

> **12 dos 14 projetos sem ressalva, 2 com ressalva real, custo R$ 0.** Duas
> linguagens da lista original (Stata e VBA) não têm caminho gratuito e saem com
> justificativa escrita.

As restrições que realmente limitam são quatro: **8 GB de RAM** (obriga rodar um
stack por vez e derruba a meta de escala do P03), **ausência de Mac** (o app iOS
fica guiado por CI, não interativo), **Unreal Engine** (8 GB + 2 GB de VRAM
provavelmente não abrem o editor) e **Triton** (exige GPU sm_70; a desta máquina
é sm_61 → Colab).

## Restrições declaradas

| Restrição | Afeta | Como foi resolvido |
|---|---|---|
| macOS obrigatório | Swift (iOS), Objective-C, Metal | CI em runner `macos-latest` do GitHub Actions |
| GPU NVIDIA | CUDA | Benchmark em instância cloud; demais APIs rodam local |
| Licença paga | MATLAB, SAS, Stata, Wolfram, ABAP, RPG, PL/I | Octave, tiers gratuitos, ou módulo `referência` rotulado |
| Sem placa FPGA | Verilog, VHDL | Simulação com Verilator e GHDL |
| Fora do escopo | Carbon, VBScript | Decisão justificada: Carbon não tem release estável; VBScript foi descontinuado |

## Mapa linguagem → projeto

Cobertura por grupo da lista de referência:

- **Uso geral** (13/13): distribuídas em todos os projetos
- **Sistemas e baixo nível**: P01, P09 — exceto Carbon (justificado)
- **Web**: P04
- **Mobile**: P08
- **Dados, ciência e IA**: P05
- **Funcionais** (13/13): P02, P03, P04
- **Scripting e automação**: P12
- **Bancos e consulta**: P10
- **Jogos**: P07
- **Hardware e embarcados**: P09
- **GPU e paralelo**: P06
- **Corporativo e legado**: P13
- **Blockchain**: P11
- **Lógica e prova formal**: P02
- **Educação e visuais**: P14
- **Infra e configuração**: P01, P03, P12
