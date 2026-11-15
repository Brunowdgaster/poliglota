# Contexto para retomar o trabalho

Documento de retomada. Se a sessão foi reiniciada e o histórico se perdeu,
**leia este arquivo primeiro**: ele contém o objetivo, o estado medido, as
convenções já estabelecidas e a ordem de trabalho decidida. Nada aqui precisa
ser redescoberto.

> ### ⚠ Pendência que atravessa sessões: desinstalar o ambiente
>
> O portfólio instalou cerca de **11 GB** de compiladores, interpretadores,
> imagens Docker e a *store* do Nix. **Nada disso está no git** — binários e
> *toolchains* não são código do projeto.
>
> O registro completo, com data de instalação, licença de cada item e os
> comandos de remoção, está em **[AMBIENTE.md](AMBIENTE.md)**.
>
> **Isto é uma tarefa pendente do projeto**, decidida pelo autor: quando os 14
> projetos estiverem prontos, rodar a seção *Desinstalando* daquele arquivo.
> Não remover antes — cada projeto depende do seu *toolchain* para o
> `verifica.sh` passar.
>
> Dois itens não devem ser removidos pela lista: `sqlmap` e `ffuf` foram
> instalados antes deste portfólio e não pertencem a ele.

Última atualização: 2026-11-15.

---

## 1. O objetivo, na forma em que foi pedido

> "Mostrar domínio completo e absoluto do maior número de linguagens possíveis."

Catorze projetos de engenharia que, somados, cobrem ~130 linguagens. Cada
linguagem aparece **onde é a escolha tecnicamente certa**, com resultado medido
e trade-off escrito — não como pasta com *hello world*.

### A calibração que o portfólio assume

Ninguém domina 130 linguagens de forma absoluta. O que estes repositórios
demonstram, e declaram em cada README:

- **profundidade real** em um núcleo de linguagens (hoje **23**, contadas por
  ferramenta e não à mão — ver abaixo);
- **fluência de leitura, porte e avaliação de trade-off** nas demais.

Essa distinção é deliberada e é parte do valor: um portfólio que mente sobre
profundidade é descoberto na primeira pergunta técnica.

---

## 2. Onde está, medido em 2026-11-14

### Linguagens com implementação profunda: **5**

| linguagem | projeto | evidência |
|---|---|---|
| Rust | P01, P03 | VM completa sem `unsafe`; NIF medindo ~30 ns de travessia |
| Haskell | P02 | compilador com inferência de Hindley-Milner |
| C | P01 | VM + GC + montador, gcc/clang/MSVC |
| Elixir | P03 | núcleo OTP, 700 mil conexões, 35 testes |
| Go | P03 | gateway WebSocket sem dependências, 7,1 KB/conexão |
| Erlang | P03 | conexão alternativa na mesma árvore; 1 byte de diferença |
| Gleam | P03 | codec tipado, 13 testes, `@external` para OTP sem FFI |
| Scala | P03 | Loom contra BEAM: 1.323 vs 2.732 bytes, inanição medida |
| Clojure | P03 | atom/ref/agent contra GenServer; 2 patologias achadas |
| HCL | P03 | OpenTofu, 20 asserções verificando 5 propriedades |
| Nim | P01 | porte da VM em **255 linhas**, 9/9, ORC do runtime |
| Odin | P01 | porte da VM, 9/9, `virtual.Arena` e `context.allocator` |
| D | P01 | porte da VM, 9/9, `SumType` exaustivo |
| V | P01 | porte da VM, 9/9, handle+arena imposto pelo tipo-soma |
| Ada | P01 | porte da VM, 9/9, subtipos restritos e `Unchecked_Conversion` |
| OCaml | P02 | porte do verificador, **23/23 byte a byte**, 604 linhas |
| F# | P02 | porte do verificador, 23/23, **496 linhas** — menos que o Haskell |
| Perl | P12 | `lint_medicoes.pl`: número publicado sem dispersão ao lado |
| AWK | P12 | `estatistica.awk`: faixa, dispersão e a regra de decisão |
| Lua | P12 | `cobertura.lua`: esta própria contagem |
| Raku | P12 | `gramatica_fasm.raku`: **9/9 byte a byte** contra o desmontador em C |
| Tcl | P12 | `dirige_repl.exp`: 12 interações em dois REPLs, com Expect |
| Nix | P12 | `flake.nix`: 4 devShells; achou um bug que só um 2º ambiente acha |

> A contagem agora é feita por `p12-toolbelt/lua/cobertura.lua`, não à mão.
> Ela corrigiu meu número na primeira execução — eu vinha dizendo 17, e são 20,
> porque eu não contava as três ferramentas que acabara de escrever. E resolve
> a **sobreposição**: Rust conta uma vez no total e duas na soma por projeto.
>
>     lua5.4 lua/cobertura.lua lua/registro.lua --roadmap ../poliglota/ROADMAP.md

### Auxiliares genuinamente usadas: 4

Bash (1.010), YAML/Actions (259), PowerShell (100), CMake (46). Aparecem no
plano (P12 e infra), mas não contam como "dominadas": são ferramenta, não
produto.

### Criadas do zero: 2

`.fasm` (390 linhas em 12 programas) e `forge` (132 linhas em 24 programas) —
projetadas **e** implementadas, com gramática, montador e compilador.

### Progresso contra o plano

| projeto | linguagens-chave | feitas | falta |
|---|---|---|---|
| P01 metal-vm | 10 | 2 (C, Rust) | C++, Zig, Assembly ×3, Nim, Odin, D, V, Ada |
| P02 forge-lang | 11 | 1 (Haskell) | OCaml, F#, Lisps, Prolog, Lean, Coq, Agda, Idris, TLA+, WAT |
| P03 otp-mesh | 8 | **8 — completo** | — |
| P04–P14 | ~95 | 0 | tudo |

**5 de ~130 — 3,8%.**

---

## 3. Repositórios publicados

Todos públicos, licença MIT, CI verde, na conta `Brunowdgaster`.

| repo | o que é | estado |
|---|---|---|
| [poliglota](https://github.com/Brunowdgaster/poliglota) | índice, roadmap, viabilidade, este arquivo | — |
| [metal-vm](https://github.com/Brunowdgaster/metal-vm) | VM de bytecode em C e Rust | 9 casos, 54/54 cross-compat, CI 9/9 |
| [forge-lang](https://github.com/Brunowdgaster/forge-lang) | linguagem funcional + compilador | 39/39, closures, CI verde |
| [otp-mesh](https://github.com/Brunowdgaster/otp-mesh) | plataforma OTP + gateway Go | 10/10 + 7/7, CI 3/3 |

Pastas locais: `C:\Users\bruno\Desktop\Projetos\para se gabar\{poliglota,p01-metal-vm,p02-forge-lang,p03-otp-mesh}`,
mais `p04-*` a `p14-*` vazias.

---

## 4. A ordem de trabalho decidida

**P03 terminado** (8/8, CI 9/9) e **camada 1 do P01 entregue** (5 portes, CI
10/10). Linguagens com profundidade real: 10 → **15**.

A ordem de trabalho mudou depois de uma correção do usuário, registrada na
seção 9: cortar **por camada de custo, atravessando os projetos**, e não
projeto por projeto em profundidade.

~~Primeiro: terminar o P03.~~ Faltavam 6 linguagens (Erlang, Gleam, Rust, Scala,
Clojure, HCL) e a ponte entre o gateway em Go e o núcleo em Elixir, que o
README admite não existir.

**Depois, nesta ordem**, escolhida por **rendimento de linguagens por esforço** —
não por impressão técnica, que foi o critério do roadmap original e é o errado
para este objetivo:

| ordem | projeto | linguagens | por quê |
|---|---|---|---|
| 1 | **P12** toolbelt | 11 | peças de 50–200 linhas cada; maior rendimento do portfólio |
| 2 | **P10** query-lab | 7 | mesmo domínio em 7 dialetos de consulta |
| 3 | **P14** first-code | 5 | programas pequenos, linguagens visuais |
| 4 | **P05** datalab | 10 | mesmo modelo estatístico traduzido |
| 5 | **P06** gpu-arena | 7 | mesmo kernel em 7 APIs de GPU |
| 6 | P01 restante | 8 | porte do núcleo da VM, ~300 linhas cada |
| 7 | P04, P07, P08, P11, P13 | ~25 | produtos maiores |
| 8 | **P02 restante** | 10 | provas em Lean/Coq/Agda/Idris: o trabalho mais caro de todos |

P12 → P06 levaria de 10 para ~50 linguagens com esforço comparável ao de
terminar P02 sozinho.

---

## 5. Convenções estabelecidas — siga-as

### Em cada projeto

- **README com 9 seções**: uma frase + resultado no topo, objetivo do
  experimento, resultado medido, como rodar, arquitetura, **tabela de
  linguagens e ferramentas** (`ferramenta | onde é usada | por que ela e não
  outra | alternativa descartada`), decisões e trade-offs, limites conhecidos,
  roadmap, aprendizados, estado atual.
- **Status honesto no topo**, com tabela `✅ / ⬜` do que existe e do que não.
- **Licença MIT**, `.gitattributes` com `* text=auto eol=lf`, `.gitignore`.
- **CI no GitHub Actions**, verde antes de declarar pronto.

### Medição

- **Sempre reportar dispersão** ao lado do número. Se a dispersão é da ordem do
  efeito, o efeito **não está estabelecido** — diga isso.
- **Usar o mínimo**, não a média: no P01, o mínimo reproduziu dentro de 0,3 %
  entre sessões enquanto o máximo variava 3×.
- **Intercalar** execuções ao comparar variantes. Medir 15× uma e depois 15× a
  outra atribui a deriva da máquina à segunda.
- **Nunca comparar números de sessões diferentes.** Foi o erro da v2 do README
  do P01.
- **Runner de CI não é linha de base**: dois runners `ubuntu-latest` deram
  631 e 411 Mops/s para binários idênticos.

### Git

- Mensagens de commit em português, com corpo explicando **o porquê** e os
  números medidos.
- Terminar com `Co-Authored-By: Claude Opus 5 <noreply@anthropic.com>`.
- Publicar repo novo **só com autorização explícita** do usuário.

---

## 6. A lição que se repetiu sete vezes

**O instrumento esconde o próprio sinal.** Começou como "falha silenciosa:
redirecionar a saída e não olhar o código de retorno", e as duas últimas
ocorrências mostraram que o padrão é mais largo que isso.

| # | onde | como escondeu |
|---|---|---|
| 1 | `bench/run_bench.sh` (P01) | pulava binários inexistentes em silêncio |
| 2 | `Mesh.Chaos.kill_fraction` (P03) | devolvia contagem parcial no timeout |
| 3 | `setup.sh` (P03) | `apt-get install` sem `update` e sem checar retorno |
| 4 | `tests/smoke.sh` (P03) | `2>/dev/null` escondia o motivo da falha |
| 5 | `cmd/loadgen` (P03) | tratado **preventivamente**: sai com erro se abrir menos conexões do que pediram |
| 6 | `bench/nif.exs` (P03) | media A em bloco e B em bloco; duas execuções deram **conclusões opostas** |
| 7 | `setup-opentofu` no CI (P03) | o wrapper da action **engolia o exit code** de `plan -detailed-exitcode`: passava local, falhava no CI, e reportava "nenhum desvio" em vez de "não sei" |

**Conhecer o modo de falha não imuniza contra ele** — sete vezes é prova disso.
A conclusão prática mudou de "tome cuidado" para **"a correção tem de ser
código"**:

- cheque o retorno, mostre o motivo, falhe alto;
- **alterne** A e B ao comparar, e reporte a dispersão ao lado do número;
- **recuse-se a concluir** quando a margem não supera o ruído;
- **autoteste o instrumento** antes de usá-lo, e confronte duas evidências
  independentes. Em `infra/verifica.sh`, código de saída **e** texto do plan:
  quando discordam, ele diz "INSTRUMENTO QUEBRADO", não "nenhum desvio".

**Falso OK é pior que falha.**

Outras lições já pagas:

- **Teste instável é sintoma do sistema, não do teste.** `Mesh.close/1`
  retornava antes de o `Registry` convergir.
- **Verificar capacidade, não identidade de pacote**: `command -v docker`, não
  `dpkg -s docker.io`.
- **O instrumento tem viés**: RSS conta só páginas tocadas, então buffer grande
  e ocioso é invisível. Anomalia implausível na tabela é informação.
- **Comentar uma invariante encontra bugs**: o bug de slot do P02 apareceu
  enquanto eu escrevia por que a invariante valia — e não valia.

---

## 7. Ambiente da máquina — fatos verificados

> **O inventário do que foi instalado, e como desinstalar, está em
> [AMBIENTE.md](AMBIENTE.md).** Esta seção descreve as *limitações* da
> máquina; aquela descreve o que foi *acrescentado* a ela.

| | |
|---|---|
| CPU | Intel i5-8250U, 4C/8T, classe U com throttling |
| RAM | 7,9 GB na máquina, **o WSL2 vê 3,8 GB** (metade do host, padrão sem `.wslconfig`) |
| GPU | Intel UHD 620 + **NVIDIA MX150, compute capability 6.1** |
| Disco | 610 GB livres |
| SO | Windows 11 Home + WSL2 Ubuntu 26.04, `systemd` PID 1, cgroup v2 |

### Armadilhas conhecidas desta máquina

- **Smart App Control bloqueia binários do MSVC.** O `fvm.exe` do P01 é
  bloqueado em qualquer pasta (CodeIntegrity 3077/3118), enquanto hello-world
  do mesmo compilador e binários Rust rodam. Solução adotada: compilar no WSL.
- **`perf` funciona no WSL2** com contadores de hardware reais
  (`perf_event_paranoid = 2`, PMU exposta). Já usado no P01.
- **CUDA funciona no WSL2** (`nvidia-smi` responde, `/dev/dxg` presente), mas
  **Triton exige compute capability ≥ 7.0** e a MX150 é 6.1 → Colab.
- **Portas efêmeras 32768–60999** limitam testes de socket em loopback a
  ~28 mil conexões.
- **`ulimit -n` 10.240** por padrão; o teto duro é 1.048.576.
- **Heredoc do Bash + Python colapsa barras invertidas.** Escrever scripts com
  `\\` ou `\a` dentro de heredoc corrompe o arquivo. Use a ferramenta Write
  para criar o script e então executá-lo.

### Toolchains instalados e verificados

C/C++ (gcc 15.2 no WSL, MSVC 19.5 no Windows), Rust 1.93, GHC 9.10.3,
Erlang/OTP 27, Elixir 1.18.3, rebar3, Gleam 1.19.2, Go 1.26.8, OpenJDK 25,
scala-cli 1.19, Clojure 1.12, Docker Engine 29.1, kind 0.33, kubectl 1.37,
OpenTofu 1.13.9, NATS 2.15.3, perf, cmake, `gh` CLI **autenticado como
Brunowdgaster**.

`p03-otp-mesh/setup.sh` reprovisiona tudo; `tests/smoke.sh all` verifica
(11 checagens, compila e executa em cada toolchain).

### Escolhas de licença já decididas

| escolhido | em vez de | motivo |
|---|---|---|
| Docker **Engine** | Docker Desktop | Desktop exige licença paga acima de certo porte |
| **OpenTofu** (MPL-2.0) | Terraform | virou BUSL em 2023: *source-available*, não livre |
| **NATS** (Apache-2.0) | Kafka / Redpanda | 15 MB e livre; Redpanda é BUSL |
| **providers locais** | AWS/GCP/Azure/Oracle | todas pedem cartão mesmo na camada gratuita |

**Sem caminho gratuito, a excluir do plano com justificativa escrita:**
Stata (não há edição livre) e VBA (não há Office nesta máquina;
LibreOffice Basic é outro dialeto).

---

## 8. O P03 está completo: o que ele mediu

**8 de 8 linguagens integradas, CI 9/9.** Nada aqui deve ser refeito.

| peça | linguagem | resultado |
|---|---|---|
| núcleo OTP | Elixir | 700 mil conexões, 3.534 B cada, 35 testes |
| gateway WebSocket | Go | 7,1 KB/conexão, buffers de 1 KiB economizam 35% |
| ponte gateway↔núcleo | Elixir + Go | `packet: 4`, 500 sockets reais viram 500 processos |
| conexão alternativa | Erlang | **1 byte** de diferença contra Elixir |
| codec tipado | Gleam | 3 erros movidos para o compilador; `@external` sem FFI |
| codec como NIF | Rust | travessia **~30 ns**; Elixir ganha até 4 KiB, resto no ruído |
| concorrência | Scala + JDK 25 | Loom: **1.323 B** vs 2.732 B da BEAM, mas **inanição total** sob carga de CPU (BEAM degrada 21x) |
| contenção | Clojure | `agent` × GenServer: **não determinado** (faixas se sobrepõem); sem coordenação perde 70% em silêncio |
| infraestrutura | HCL + OpenTofu | 20 asserções: convergência, idempotência, desvio, grafo, destroy |

**Protocolo da ponte verificado em 4 linguagens** contra `protocolo/vetores.txt`:
Elixir, Go, Gleam e Rust.

Dois achados que vieram de números implausíveis, não de hipóteses:

- `write_concurrency` do ETS fragmenta **por chave**: na mesma chave,
  adicionar núcleos deixa **mais lento**, e chaves separadas escalam — **três
  ordens de grandeza** de diferença na mesma tabela. Medido com repetições em
  `bench/contencao.exs`;
- `cast` mais lento que `call` é **falta de contrapressão**: a caixa chegou a
  811 mil mensagens. `message_queue_data: :off_heap` rende **~66x** sob
  contenção (3 repetições) e custa 2,8x sem ela. `call` é auto-limitado — dá
  contrapressão de graça.

> **Estas duas linhas estavam defasadas**, e quem achou foi
> `p12-toolbelt/perl/lint_medicoes.pl` rodando sobre este arquivo. Elas
> carregavam **1.400x** (ETS) e **254x** (`off_heap`), que são os números
> **pré-auditoria** — eu corrigi o README do P03 para ~912x e ~66x e esqueci o
> índice.
>
> É exatamente o caso de uso da ferramenta: número publicado sem dispersão ao
> lado é número que ninguém revisita.

**Auditoria completa das medições** em `p03-otp-mesh/docs/AUDITORIA.md`: toda
afirmação numérica do README revisada, **7 corrigidas**. Parte das medições de
sessões anteriores rodou com carga de fundo na máquina (jogo e navegador no
Windows, que o `/proc/loadavg` do WSL2 não vê). Nenhuma correção inverteu
conclusão qualitativa; quatro mudaram o número, uma virou "não determinado", uma
ficou mais forte, e uma revelou número **condicional** sem a condição
declarada.

O instrumento ganhou `bench/calibra.sh` (detecta máquina ocupada por tempo de
laço fixo), o modo `--esperar` (distingue "meu benchmark aqueceu a CPU" de
"há carga externa") e a **regra de sobreposição de faixas**.

Outros resultados medidos:

- 700 mil conexões, 3.783 bytes cada, num nó BEAM;
- caos: o supervisor sobrevive e o `Stats` volta em milissegundos com a
  contagem relida do `Registry`. Os **tempos** foram refeitos na auditoria:
  convergência de 1,8–4,6 s para 60 mil mortes (dispersão 155%), e reinício do
  `Stats` **condicional** — 6 ms em sistema quieto, 19–37 ms sob agitação;
- difusão particionada ~3× a serial, custando +7 % de memória por conexão;
- socket WebSocket real: 7,1 KB por conexão, 1,9× o processo BEAM;
- buffers de 1 KiB economizam 35 % contra os 4 KiB padrão das bibliotecas.
