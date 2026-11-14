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
| P01 | **[metal-vm](https://github.com/Brunowdgaster/metal-vm)** | VM de bytecode em implementações comparadas sob medição | C, C++, Rust, Zig, Assembly ×3, Nim, Odin, D, V, Ada | 🟨 **C e Rust completos.** 4 configurações × 3 CPUs, mecanismo confirmado com contadores de hardware (`perf`), 42/42 na compatibilidade cruzada de bytecode, CI 9/9. C++/Zig/Assembly pendentes |
| P02 | `forge-lang` | Linguagem própria + prova formal do type checker | Haskell, OCaml, F#, Racket, Scheme, CL, Clojure, Prolog, Lean, Coq, Agda, Idris, Isabelle, TLA+, WAT | ⬜ scaffold |
| P03 | `otp-mesh` | Plataforma distribuída de tempo real, chaos-tested | Elixir, Erlang, Gleam, Go, Rust, Scala, Clojure, HCL, YAML | ⬜ scaffold |
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

## O que o P01 já estabeleceu

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

Também o que **não** deu certo: três versões da conclusão principal foram
publicadas e corrigidas conforme novas máquinas apareceram, e o `git log`
mostra as três. Isso é parte do que o portfólio demonstra.

## Por onde começar a ler

- Quer ver **engenharia de sistemas**: P01 → P09 (a VM roda na CPU que eu projetei).
- Quer ver **rigor teórico**: P02 (type checker provado correto em Lean).
- Quer ver **produto entregável**: P04 e P03.
- Quer ver **algo que quase ninguém faz**: P13 (COBOL → C# com equivalência provada).

## Ordem de construção

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
