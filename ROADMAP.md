# Portfólio Poliglota — Roadmap

Meta: um conjunto de projetos que cobre a lista inteira de linguagens citada, de forma **defensável em entrevista** — cada linguagem aparece onde ela é a escolha certa, não como enfeite.

## Princípio de design (leia antes de tudo)

Existem duas formas de "cobrir 130 linguagens":

<!-- lint-medicao: ignorar — prosa do plano, sem número medido -->
1. **130 pastas com hello-world.** Cobre a lista, prova zero. Qualquer revisor identifica em 30 segundos.
2. **14 projetos reais onde cada linguagem entra por um motivo técnico**, com benchmark, trade-off escrito e CI que compila tudo. Isso prova engenharia, não só sintaxe.

Este roadmap é a opção 2. O que vende não é "sei Zig", é **"implementei o mesmo VM core em C, Rust e Zig, medi, e aqui está por que o Zig ganhou em tempo de compilação e perdeu em ecossistema"**.

Nota honesta: ninguém domina 130 linguagens de forma absoluta — nem os autores delas. O que este portfólio demonstra de verdade é (a) **profundidade real** em ~12 linguagens-núcleo e (b) **fluência de leitura e porte** nas outras ~120, o que é raro e muito mais valioso que a alegação vazia. Os READMEs devem declarar isso: honestidade calibrada é diferencial, não fraqueza.

---

## Estrutura do portfólio

Um repositório por projeto (melhor para GitHub: 14 repos com stars próprias > 1 monorepo gigante), mais um repo-índice `poliglota` que é o hub: matriz de cobertura, benchmarks comparativos e links.

```
poliglota/                 <- repo-índice (o que o recrutador abre primeiro)
p01-metal-vm/
p02-forge-lang/
p03-otp-mesh/
p04-webstack/
p05-datalab/
p06-gpu-arena/
p07-engine/
p08-tri-mobile/
p09-silicon/
p10-query-lab/
p11-chain/
p12-toolbelt/
p13-legacy-bridge/
p14-first-code/
```

---

## Os 14 projetos

### P01 — `metal-vm` : VM de bytecode + runtime de baixo nível

**O que é:** uma máquina virtual de pilha (tipo Lua/CPython simplificada) com GC, JIT simples e loop de interpretação otimizado. Implementada 3x para comparação direta.
<!-- lint-medicao: ignorar — "3x" aqui é CONTAGEM (três vezes), não razão -->

**Linguagens:** C, C++, Rust, Zig, Assembly x86-64, Assembly ARM64, Assembly RISC-V, Nim, Odin, D, V, Ada, Makefile, CMake, Nix

**Por que cada uma entra:** C é a baseline canônica de VMs; C++ mostra RAII/templates no mesmo problema; Rust mostra ownership num GC (o caso difícil); Zig mostra `comptime` e alocadores explícitos; Assembly entra no *dispatch loop* (computed goto vs. jump table em asm puro) com medição; Nim/Odin/D/V/Ada são portes do **núcleo mínimo** (~300 linhas) com ensaio comparativo — e o README diz claramente que são portes, não produção.

**Entregável que impressiona:** gráfico de benchmark (ops/s) das 3 implementações completas + tabela "linhas de código × performance × tempo de compilação".

### P02 — `forge-lang` : linguagem própria, com prova formal

**O que é:** uma linguagem funcional tipada com inferência Hindley-Milner, compilando para o bytecode do P01 e para WebAssembly. Mais a **prova de que o type checker é correto**.

**Linguagens:** Haskell (front-end/type checker), OCaml (porte do checker), F#, Racket, Scheme, Common Lisp, Clojure, WebAssembly (WAT), Prolog, Lean 4, Coq/Rocq, Agda, Idris, Isabelle, TLA+

**Por que cada uma entra:** Haskell/OCaml/F# são as linguagens *em que compiladores são escritos* — implementar o mesmo checker nas três é um dos exercícios mais reveladores que existem; Racket/Scheme/Common Lisp dão o interpretador meta-circular e macros (homoiconicidade); Prolog expressa unificação de tipos declarativamente (o algoritmo *é* Prolog); Lean/Coq/Agda/Idris provam soundness (progress + preservation); Isabelle faz a versão clássica; TLA+ especifica o runtime concorrente. WAT é backend real, escrito à mão primeiro.

**Entregável que impressiona:** `theorems/` com progress & preservation provados em Lean, linkados do README para o código que eles garantem. Isso coloca você no percentil 99.

### P03 — `otp-mesh` : plataforma distribuída de tempo real

**O que é:** um sistema de presença/mensageria com milhões de conexões simuladas, supervisão, back-pressure e particionamento.

**Linguagens:** Elixir, Erlang, Gleam, Go, Rust, Scala, Clojure, YAML (k8s), HCL (Terraform), Dockerfile, Nix, Bash

**Por que cada uma entra:** Erlang/OTP é o estado-da-arte real em tolerância a falhas (supervisores, hot code reload); Elixir é a camada produtiva sobre a mesma BEAM; Gleam adiciona tipagem estática na BEAM (mostra que você entende a VM, não só a sintaxe); Go é o gateway (goroutines + net/http, onde ele é imbatível); Rust é o caminho crítico de serialização; Scala/Clojure fazem o processamento de stream (Akka/Flink e transducers); HCL + YAML provisionam.

**Entregável que impressiona:** chaos test — matar processos e mostrar recuperação em GIF, com latência p99 antes/depois.

### P04 — `webstack` : produto web full-stack, com 3 front-ends

**O que é:** um produto real (ex.: rastreador de orçamento colaborativo), com o **mesmo front-end reimplementado em 3 paradigmas** e um módulo legado PHP sendo estrangulado.

**Linguagens:** TypeScript, JavaScript, HTML, CSS, Sass/SCSS, Elm, PureScript, PHP, Ruby, Dart, GraphQL, CoffeeScript

**Por que cada uma entra:** TS/React é o mainstream (precisa estar lá e ser excelente); Elm prova arquitetura MVU com zero runtime exception; PureScript mostra Haskell no browser; Dart/Flutter Web é a quarta via; PHP é o legado honesto (Laravel) com o padrão strangler-fig documentado; Ruby/Rails é o painel admin (onde Rails ainda é o mais rápido de entregar); GraphQL é o contrato entre tudo; CoffeeScript aparece em 1 arquivo *arqueológico* com nota explicando por que morreu — contexto histórico também é domínio.

**Entregável que impressiona:** tabela comparando bundle size, erros em runtime e velocidade de desenvolvimento entre TS/Elm/PureScript, no mesmo produto.

### P05 — `datalab` : pipeline de dados e ML poliglota

**O que é:** um estudo real ponta a ponta (ex.: previsão de série temporal de consumo elétrico), onde cada etapa usa a ferramenta que a comunidade científica realmente usa.

**Linguagens:** Python, R, Julia, Scala (Spark), Fortran, Mojo, MATLAB/Octave, SAS, Stata, Wolfram, SQL

**Por que cada uma entra:** Python é o orquestrador e o ML (PyTorch); R faz a estatística séria (modelos mistos, ggplot — onde R é superior ao Python); Julia resolve o ODE/otimização numérica com velocidade de C e sintaxe de script; Fortran é o kernel numérico legado que você *chama* do Python via f2py (mostra interop, não nostalgia); Scala/Spark é a escala distribuída; Mojo mostra a aposta nova com benchmark honesto; Octave/SAS/Stata/Wolfram aparecem como **traduções do mesmo modelo** com ensaio de equivalência — exatamente o que consultoria e academia pedem.

**Entregável que impressiona:** o mesmo modelo estatístico, 5 linguagens, mesmos coeficientes, tabela de tempo de execução e legibilidade.

### P06 — `gpu-arena` : o mesmo kernel em todas as APIs de GPU

**O que é:** N-body (ou matmul tiled) implementado em todas as linguagens de GPU, com harness de benchmark unificado.

**Linguagens:** CUDA, OpenCL, Triton, WGSL, GLSL (compute), HLSL (compute), Metal Shading Language, C++, Python

**Por que cada uma entra:** é literalmente o mesmo problema resolvido em 7 modelos de programação paralela — o README explica memória compartilhada, warps/wavefronts, occupancy e por que cada API expõe isso de forma diferente. Triton mostra o estado atual de kernels para IA.

**Entregável que impressiona:** roofline plot + tabela GFLOP/s por API e por hardware.

**Restrição:** CUDA exige GPU NVIDIA; Metal exige macOS. Ver seção Restrições.

### P07 — `engine` : motor de jogo + jogo

**O que é:** um motor 2.5D próprio em C++ com camada de script, e o mesmo jogo prototipado em Godot e Unreal para comparação de produtividade.

**Linguagens:** C++, C#, Lua, GDScript, Unreal Blueprints, HLSL, GLSL

**Por que cada uma entra:** C++ é o motor (ECS, render loop); C# é a camada de gameplay (embedding de .NET ou porte Unity); Lua é o sistema de mods (sandbox + hot reload — o uso canônico de Lua); GDScript é o protótipo rápido no Godot; Blueprints mostram que você entende programação visual de verdade (grafos de execução) e não a despreza; HLSL/GLSL são os shaders.

**Entregável que impressiona:** devlog com GIFs + ensaio "motor próprio vs. Godot vs. Unreal: quando cada escolha se paga".

### P08 — `tri-mobile` : o mesmo app, três vezes

**O que é:** um app offline-first com sync (ex.: diário de treino), nativo Android, nativo iOS e cross-platform — para comparar honestamente.

**Linguagens:** Kotlin, Java, Swift, Objective-C, Dart

**Por que cada uma entra:** Kotlin/Compose é o Android moderno; Java entra no módulo legado + interop JNI (realidade de qualquer app antigo); Swift/SwiftUI é o iOS moderno; Objective-C é a ponte de interop com uma lib antiga; Dart/Flutter é a terceira via, com medição de tamanho de binário, tempo de startup e fidelidade de UI.

**Entregável que impressiona:** tabela LOC/startup/tamanho + conclusão argumentada.

**Restrição:** build iOS exige macOS — usar runner `macos-latest` do GitHub Actions para o CI compilar.

### P09 — `silicon` : hardware, FPGA e embarcado

**O que é:** um núcleo RISC-V mínimo (RV32I) em Verilog, verificado, rodando o bytecode do P01; mais firmware em 3 linguagens.

**Linguagens:** Verilog, SystemVerilog, VHDL, Assembly RISC-V, C, Rust (embedded/embassy), MicroPython, Arduino C++, Makefile, Tcl

**Por que cada uma entra:** Verilog descreve o core; SystemVerilog faz o testbench/verificação (assertions, cobertura) — a parte que separa amador de engenheiro; VHDL reimplementa um módulo (ALU) para mostrar as duas culturas; Tcl é a linguagem real de tooling de EDA (Vivado/Quartus); Rust embedded mostra async sem RTOS; MicroPython e Arduino mostram o espectro produtividade↔controle no mesmo periférico.

**Entregável que impressiona:** waveform + "meu VM do P01 rodando na minha CPU, no meu ISA". Verticalização total, do transistor à linguagem. É a peça mais forte do portfólio.

### P10 — `query-lab` : dados e linguagens de consulta

**O que é:** o mesmo domínio (grafo social + transações) modelado em relacional, grafo e RDF, com a mesma pergunta respondida nas 4 linguagens e benchmark.

**Linguagens:** SQL (ANSI), PL/pgSQL, PL/SQL, T-SQL, Cypher, SPARQL, GraphQL, Python

**Por que cada uma entra:** mostra que você entende *modelos de dados*, não só dialetos — quando o JOIN recursivo perde para o Cypher, e o que o SPARQL faz que nenhum dos dois faz (inferência ontológica). Os três dialetos procedurais aparecem na mesma stored procedure, traduzida, com nota sobre as diferenças de transação e tratamento de erro de cada fornecedor.

**Entregável que impressiona:** a mesma query, 4 linguagens, `EXPLAIN` de cada, tempos medidos.

### P11 — `chain` : contratos inteligentes com testes diferenciais

**O que é:** um AMM/cofre simples implementado em Solidity **e** Vyper, com fuzzing diferencial provando que se comportam igual; mais portes para outras VMs.

**Linguagens:** Solidity, Vyper, Move, Cairo, Rust (Solana/Anchor), TypeScript (testes)

**Por que cada uma entra:** Solidity é o padrão; Vyper é a mesma lógica com modelo de segurança diferente (e o diff *é* o conteúdo); Move mostra recursos lineares (o modelo que previne double-spend pelo sistema de tipos); Cairo mostra ZK/provas; Rust/Anchor é o modelo de contas da Solana.

**Entregável que impressiona:** relatório de auditoria próprio + fuzzing diferencial verde.

### P12 — `toolbelt` : automação e scripting

**O que é:** um conjunto de ferramentas que você realmente usa (deploy, backup, análise de log, automação de desktop), cada uma na linguagem que é a escolha natural naquele terreno.

**Linguagens:** Bash, PowerShell, Perl, AWK, Lua, Tcl, Raku, Groovy, AutoHotkey, VBA, Makefile, Nix, Dockerfile, YAML, HCL

<!-- lint-medicao: ignorar — "10x menos caracteres" é a INTENÇÃO do plano,
     não uma medição; quando for medido, vai para o README do p12 -->
**Por que cada uma entra:** Bash/PowerShell é o par POSIX↔Windows (mesmo script, duas culturas); AWK faz o one-liner de log que vence Python em 10x menos caracteres; Perl faz o regex pesado; Raku mostra a evolução (grammars); Groovy é Jenkins/Gradle real; Tcl é Expect/automação interativa; AutoHotkey e VBA são automação de desktop/Office — desprezados e extremamente empregáveis; Nix garante reprodutibilidade de todo o portfólio.

**Entregável que impressiona:** um `nix develop` que levanta o ambiente de *todos* os 14 projetos.

### P13 — `legacy-bridge` : modernização de legado (a peça mais rara)

**O que é:** um sistema bancário batch em COBOL, com arquivos de tamanho fixo, sendo migrado incrementalmente para .NET/C# com equivalência de regra de negócio provada por testes.

**Linguagens:** COBOL (GnuCOBOL), Delphi/Object Pascal, Visual Basic .NET, C#, PL/I, RPG, ABAP, Apex, SQL

**Por que cada uma entra:** COBOL ainda roda a maior parte do dinheiro do mundo e quase nenhum júnior toca nele — vantagem competitiva pura; Delphi é o cliente desktop legado; VB.NET é a primeira etapa de migração; C# é o destino; PL/I, RPG, ABAP e Apex entram como **módulos equivalentes documentados** (cada um com 1 regra de negócio traduzida) e ensaio sobre o ecossistema de cada um (mainframe, IBM i, SAP, Salesforce).

**Entregável que impressiona:** suíte de testes que roda a regra no COBOL e no C# e compara a saída byte a byte. É exatamente o que bancos pagam caro para alguém saber fazer.

### P14 — `first-code` : currículo de programação visual

**O que é:** um material de ensino real (para criança/iniciante) implementado nas linguagens educacionais, com a mesma progressão de conceitos.

**Linguagens:** Scratch, Blockly, Logo, Smalltalk, Pascal

**Por que cada uma entra:** não é enchimento se for tratado como **design pedagógico**: Logo ensina recursão por geometria; Scratch/Blockly ensinam eventos e concorrência sem sintaxe; Smalltalk é o ambiente vivo (e o berço de OO — ler Smalltalk muda como você escreve OO); Pascal traz o rigor estruturado. Inclui um Blockly embarcado que gera código do P02.

**Entregável que impressiona:** o próprio site do currículo, publicado e usável por alguém real.

---

## Cobertura da lista original

| Grupo da lista | Coberto em | Pendências / notas |
|---|---|---|
| Uso geral | P01–P14 (todas as 13) | — |
| Sistemas / baixo nível | P01, P09 | **Carbon** fica fora: experimental, sem release estável. Documentar a decisão. |
| Web | P04 | CoffeeScript só como arqueologia (correto) |
| Mobile | P08 | iOS / Objective-C exigem macOS (CI) |
| Dados e IA | P05 | MATLAB → Octave; SAS/Stata → código + nota de licença |
| Funcionais | P02, P03, P04 | Todas as 13 cobertas |
| Scripting | P12 | VBScript substituído por VBA (VBScript foi descontinuado no Windows) |
| Bancos e consulta | P10 | Todas |
| Jogos | P07 | Todas |
| Hardware e embarcado | P09 | Todas |
| GPU e paralelo | P06 | CUDA precisa de NVIDIA; Metal precisa de macOS |
| Corporativo e legado | P13 | ABAP/RPG/PL/I como módulos equivalentes (ambientes proprietários) |
| Blockchain | P11 | Todas |
| Lógica e prova formal | P02 | Todas |
| Educação e visuais | P14 | Todas |
| Infra e configuração | P01, P03, P12 | Todas |

## Restrições reais (declarar nos READMEs, não esconder)

- **macOS obrigatório:** Swift (iOS), Objective-C, Metal. Solução: runners `macos-latest` do GitHub Actions compilam no CI — grátis em repositório público. Documentar isso conta a favor.
- **Licença paga:** MATLAB, SAS, Stata, Wolfram, ABAP, RPG (IBM i), PL/I. Solução: Octave, versões gratuitas/comunidade (SAS OnDemand, Wolfram Engine free, SAP BTP trial) e, onde não há, código + teste de mesa com ensaio de equivalência, claramente rotulado como tal.
- **Hardware:** CUDA exige GPU NVIDIA; FPGA pode ser simulado (Verilator/GHDL) sem placa.
- **Carbon e VBScript:** não valem o esforço. O README explica por quê — julgamento técnico também é entregável.

## Ambiente de desenvolvimento (estado atual da máquina)

Já instalado: `git`, `node`, `python`, `rustc/cargo`, `java` (apenas runtime), `cmake`, `perl`.

Falta instalar, em ordem de retorno:

1. **WSL2 + Ubuntu** — destrava gcc/clang, GnuCOBOL, GHC, OCaml, Verilator, GHDL, Elixir/Erlang e praticamente todo o resto. É o passo que mais rende.
2. **Docker Engine dentro do WSL** — um container por linguagem exótica, sem
   poluir o Windows. **Engine**, não Desktop: o Desktop exige licença paga
   acima de certo porte de empresa, e o Engine é Apache-2.0, roda nativo no
   WSL2 e gasta menos RAM por não ter VM intermediária. Ver
   [VIABILIDADE.md](VIABILIDADE.md#3-docker-engine-não-docker-desktop).
3. `winget install` — Go, .NET SDK, LLVM, Make, Zig, Lua, Ruby, PHP, JDK completo (JDK, não só JRE).
4. **Nix** (dentro do WSL) — ambientes reprodutíveis por projeto; isso vira o próprio P12.

## Fases sugeridas

| Fase | Projetos | Por que nessa ordem |
|---|---|---|
| 1 | P01, P12 | Fundação: o VM é a espinha de P02 e P09; o toolbelt constrói o ambiente de todos |
| 2 | P02, P10 | Compilador + dados: alto valor, só precisa de CPU |
| 3 | P03, P04 | As peças que um recrutador entende em 10 segundos |
| 4 | P09, P06 | Hardware e GPU: as mais impressionantes tecnicamente |
| 5 | P05, P07, P11 | Domínios especializados |
| 6 | P08, P13, P14 | Dependem de macOS, licenças ou tempo — fecham a cobertura |

Regra: **nenhum projeto vai para o GitHub antes de ter README completo, CI verde e um resultado medido.** 4 repositórios excelentes valem mais que 14 abandonados — e esta ordem garante que, se você parar na fase 3, já tem portfólio.

## Template de README (todo projeto usa)

1. **Uma frase** do que é + GIF/screenshot/gráfico acima de tudo
2. **Problema** — por que isso existe, qual pergunta técnica responde
3. **Resultado** — números, logo no início (benchmark, prova, cobertura)
4. **Arquitetura** — diagrama + fluxo de dados
5. **Tabela de linguagens e ferramentas** — `Linguagem | Onde é usada | Por que ela e não outra | Alternativa descartada`
6. **Como rodar** — `nix develop` ou `docker compose up`, testado em máquina limpa
7. **Decisões e trade-offs** — a seção que mais importa; inclua o que deu errado
8. **Limites conhecidos** — o que não está implementado e por quê
9. **Aprendizados** — 5 bullets concretos, sem clichê
