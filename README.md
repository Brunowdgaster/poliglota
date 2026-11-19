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
| P01 | **[metal-vm](https://github.com/Brunowdgaster/metal-vm)** | VM de bytecode em implementacoes comparadas sob medicao | C, C++, Rust, Zig, Assembly ×3, Nim, Odin, D, V, Ada | 🟨 **7 de 12: C e Rust completos, mais 5 portes** (Nim, Odin, D, V, Ada) em 9/9 contra o mesmo `.fbc`. Tabela comparativa de linhas × build × execucao × RSS; o Nim fez em **255 linhas** e o **D tem GC e ficou agrupado com os que nao tem**. 54/54 na compatibilidade cruzada, CI 10/10. C++/Zig/Assembly pendentes Acrescentados **C++, Zig e Assembly x86-64**: o `shared_ptr` do C++ **nao coleta ciclo** (o ORC do Nim coleta, e dizer isso e mais honesto que anunciar paridade); o Zig poe o **alocador na assinatura** e deriva a tabela de operandos do enum em `comptime`; e o Assembly e o **subconjunto declarado**, sem libc, porque o plano pede o dispatch loop e opcodes de heap mediriam o alocador. O porte em C++ falhou em 4 dos 9 casos por causa de um **frame raiz** que nao esta no ISA -- so no laco quente do C. E as **tres arquiteturas de Assembly** mostram a tese de cada uma em tres linhas: `a < b` custa 3 instrucoes no x86-64, 2 no ARM64 e **1 no RISC-V**, que nao tem registrador de condicao -- e o RISC-V paga isso em `<=`, que nao tem instrucao |
| P02 | **[forge-lang](https://github.com/Brunowdgaster/forge-lang)** | Linguagem funcional com inferencia total + prova formal | Haskell, OCaml, F#, Racket, Scheme, CL, Clojure, Prolog, Lean, Coq, Agda, Idris, Isabelle, TLA+, WAT | 🟨 **6 de 15: compilador completo + 2 portes do verificador.** Hindley-Milner total em Haskell, ordem superior e aplicacao parcial, 39/39 nas 3 VMs do P01. OCaml e F# reimplementam o Algoritmo W com saida **byte a byte identica** em 23 casos -- inclusive os nomes das variaveis de tipo, o que exige gerar variaveis frescas na mesma ordem. O F# escreve em **menos linhas que o Haskell** (496 contra 533) e parte 8x a 17x mais lento. Provas formais pendentes E o **Prolog** entrou refutando a propria expectativa: o Algoritmo W *e* unificacao, mas o `=/2` de Prolog **nao faz teste de ocorrencia** -- ele aceita `t0 = t0 -> t1` e constroi um termo ciclico -- e a variante que serve, `unify_with_occurs_check/2`, **nao e o padrao**. O ganho real veio do **parser**: as DCG sao a gramatica, e o porte saiu com **338 linhas contra 494** da referencia E o **Racket** -- primeira linguagem **dinamica** do projeto -- respondeu quanto do algoritmo depende do sistema de tipos de quem o escreve: a estrutura sobrevive inteira e o porte fica **20% mais curto**, mas as structs `var` e `app` colidem com **formas de padrao do `racket/match`**, e o sombreamento transformou um teste de tipo num **curinga sem nenhum aviso** E o **Common Lisp** deu a comparacao mais precisa do projeto: ele e o Racket tem a mesma AST e a mesma inferencia, e a **unica** diferenca de desenho e que Racket tem `match` e ANSI CL **nao tem casamento de padrao nenhum** -- custa **18 linhas, 4,5%**. Menos do que eu esperava: o preco e de legibilidade, nao de volume. E o `etypecase` e **melhor** que o `match` num ponto -- nao tem curinga, entao esquecer um ramo e sempre erro |
| P03 | **[otp-mesh](https://github.com/Brunowdgaster/otp-mesh)** | Plataforma distribuida de tempo real, chaos-tested | Elixir, Erlang, Gleam, Go, Rust, Scala, Clojure, HCL, YAML | ✅ **Completo, 8/8 linguagens, CI 9/9.** 700 mil conexoes supervisionadas a 3.534 B cada; protocolo verificado byte a byte em 4 linguagens; travessia de NIF em ~30 ns; Loom custa 1.323 B mas sofre inanicao total onde a BEAM degrada 21x; `agent` de Clojure fica na mesma ordem de grandeza do GenServer; 20 asseroes de IaC. **Auditoria completa das medicoes em docs/AUDITORIA.md: 5 corrigidas.** Ambiente 100% livre |
| P04 | **[webstack](https://github.com/Brunowdgaster/webstack)** | Produto web full-stack, com 3 front-ends | TypeScript, JavaScript, HTML, CSS, Sass, Elm, PureScript, PHP, Ruby, Dart, GraphQL, CoffeeScript | ✅ **12 de 12, 29 verificacoes, CI 1/1.** Cinco linguagens renderizam a MESMA view e o HTML e comparado por `cmp` -- porque **o navegador nao liga para a linguagem**. O achado: as bibliotecas de escape **nao concordam** -- o `htmlspecialchars` do PHP escreve `&#039;` e o `CGI.escapeHTML` do Ruby escreve `&#39;`, o navegador renderiza os dois igual, e nada avisa. Por isso as cinco escrevem o escape **a mao**. E o TypeScript nao entra como "e mainstream": um arquivo com 4 bugs que o `tsc` **tem de recusar**, com os codigos medidos. As **quatro vias tipadas** -- TS, Elm, PureScript, Dart -- resolvem o mesmo problema, e a diferenca apareceu no codigo: **o Elm precisou de um harness em JavaScript** para ler arquivo, e o PureScript nao. Isso e o PRECO da garantia do Elm, nao defeito |
| P05 | **[datalab](https://github.com/Brunowdgaster/datalab)** | Mesmo modelo estatistico em N linguagens | Python, R, Julia, Scala, Fortran, Mojo, Octave, SAS, hansl, Wolfram, SQL | 🟨 **9 de 10, 11 verificacoes, CI 1/1.** O unico projeto que NAO compara byte a byte, e diz por que: ponto flutuante nao e associativo. A tolerancia vem do numero de condicao MEDIDO. E a conclusao e o oposto da esperada: **a diferenca entre ALGORITMOS e 13 ordens de grandeza maior que a diferenca entre linguagens** -- duas linguagens rodando QR concordam em 1,4e-08, e a MESMA linguagem rodando equacoes normais erra 3,4e-03. O SQL, que nao tem escolha de algoritmo, erra **40% no erro padrao residual** -- e nem o R nem o Octave emitem aviso algum E o **Julia** refinou a tese: as tres implementacoes de equacoes normais -- Gauss, Cholesky e Cramer -- erram por uma **ordem de grandeza de diferenca entre si** (3,4e-03, 2,4e-02, 4,0e-01), entao "nao use equacoes normais" e conselho incompleto: **qual fatoracao se usa depois ainda muda o resultado por 100x** E o **Scala** fechou o experimento pela outra ponta: os MESMOS dois algoritmos do Fortran, escritos a mao numa JVM, dao **os mesmos numeros** (1,4e-08 e 3,4e-03). Entao mesmo algoritmo em linguagens diferentes da o mesmo erro, e mesma familia com fatoracoes diferentes erra a 100x de distancia -- a tese demonstrada nas duas direcoes E o **Mojo** achou o LIMITE da tese: mesmo algoritmo que Fortran e Scala, mesmo numero no caso bem (1,4e-14) e **3,5x pior** no mal (1,2e-02) -- porque ali XtX e **numericamente singular** (k=5,5e+19 > 1/eps) e o proprio erro deixa de ser reprodutivel. A suite ja exigia DIVERGENCIA em vez de um valor, por cautela; o Mojo provou que a cautela era necessaria |
| P06 | `gpu-arena` | Mesmo kernel em 7 APIs de GPU | CUDA, OpenCL, Triton, WGSL, GLSL, HLSL, Metal | ⬜ scaffold |
| P07 | `engine` | Motor próprio vs. Godot vs. Unreal | C++, C#, Lua, GDScript, Blueprints, HLSL, GLSL | ⬜ scaffold |
| P08 | `tri-mobile` | Mesmo app: Android nativo, iOS nativo, Flutter | Kotlin, Java, Swift, Objective-C, Dart | ⬜ scaffold |
| P09 | `silicon` | CPU RISC-V em Verilog rodando o VM do P01 | Verilog, SystemVerilog, VHDL, Assembly, C, Rust, MicroPython, Tcl | ⬜ scaffold |
| P10 | **[query-lab](https://github.com/Brunowdgaster/query-lab)** | Mesmo dominio em relacional, grafo e RDF | SQL, PL/pgSQL, PL/SQL, T-SQL, Cypher, SPARQL, GraphQL, Python | ✅ **Completo, 8 de 8, 28 testes, CI 1/1.** Uma pergunta -- "quem esta a ate 3 saltos de amizade de Ana?" -- respondida identicamente por seis implementacoes e comparada byte a byte. Ela expoe que **cada modelo ganha num eixo diferente**: o SPARQL faz o fecho ILIMITADO que o SQL so alcanca com CTE recursivo, e **nao consegue limitar** a repeticao, porque o `{n,m}` saiu da recomendacao final -- uma afirmacao que eu tinha escrito errado e corrigi medindo. No GraphQL a pergunta e **inexpressavel**, e isso esta medido, nao alegado: o aninhamento a mao devolve 14 nos, 6 nomes e **a propria origem de volta**, contra a resposta certa de 5 pessoas. E os quatro `ERRCODE` que eu escolhi colidiam com os do PostgreSQL: **`P0004` e `assert_failure`, que `WHEN OTHERS` se recusa a capturar** -- e os testes passavam em tres das quatro colisoes, porque conferiam o mesmo codigo errado que a procedure levantava E os **tres dialetos procedurais** estao lado a lado em `COMPARACAO.md`: um `ROLLBACK` aninhado no T-SQL **destroi a transacao do chamador** (medido), o `SELECT INTO` do Oracle e estrito **por padrao** -- o que o torna o mais rigoroso E o mais curto, 3 linhas contra 8 -- e **todo DDL do Oracle faz COMMIT**, levando o DML pendente com ele. O esquema ANSI roda em **quatro** motores sem uma linha de dialeto; a carga roda em tres, porque `VALUES` multilinha esta no SQL-92 e o Oracle **nao aceita** |
| P11 | `chain` | Mesmo contrato em Solidity e Vyper, fuzzing diferencial | Solidity, Vyper, Move, Cairo, Rust | ⬜ scaffold |
| P12 | **[toolbelt](https://github.com/Brunowdgaster/toolbelt)** | Automacao e higiene do proprio portfolio | Bash, PowerShell, Perl, AWK, Lua, Tcl, Raku, Groovy, AutoHotkey, Nix | ✅ **11 de 11, 58 testes (49 POSIX + 9 Windows), CI 3/3.** O AutoHotkey le o desktop do Windows e **nomeia** o que contamina uma medicao -- a primeira execucao pegou `chrome.exe` a 103% com um video no titulo, que e literalmente o cenario que corrompeu uma medicao do P03. O **VBA e o primeiro item genuinamente bloqueado do portfolio**: exige Office, que e pago. Cada ferramenta resolve um problema que o portfolio TEVE. A grammar em Raku da **9/9 byte a byte** contra o desmontador em C do P01 -- e achou que o formato aceita hexadecimal, o que eu nao sabia. O Expect achou que o OCaml emite bytes DIFERENTES num terminal e num pipe. E o flake do Nix achou que o padrao de um teste meu quebrava entre OCaml 5.3 e 5.4 -- **um segundo ambiente encontra as suposicoes que o primeiro nunca questiona** | O **VBA foi substituido por LibreOffice Basic** (livre): `Cobertura.bas` monta a cobertura do portfolio como planilha AUDITAVEL, com `=SUM()` de verdade -- e a macro LE DE VOLTA o valor que o motor de calculo produziu, para conferir com o contador em Lua
| P13 | **[legacy-bridge](https://github.com/Brunowdgaster/legacy-bridge)** | Modernizacao de legado (a peca mais rara) | COBOL, Object Pascal, VB.NET, C#, PL/I, RPG, ABAP, Apex, SQL | 🟨 **5 de 9, 22 verificacoes, CI 1/1.** Um lote bancario em COBOL migrado para C# e VB.NET, com a equivalencia provada por **`cmp`** -- e nao por tolerancia, porque dinheiro e decimal de ponto fixo e a resposta certa e UMA SO. O modo `--double` e a mesma regra com UM TIPO trocado, e **perde um centavo** em 2 de 14 contas. E minha previsao falhou primeiro: as 12 contas escolhidas a mao nao exibiam a divergencia, e um programa varreu 2000 saldos x 2500 taxas para achar onde ela aparece. **E o Object Pascal mostrou algo melhor:** o `--double` dele CONCORDAVA com o COBOL, e por acidente -- o FPC avalia `Integer / <literal real>` em precisao SIMPLES, com erro 4,7e-08, e isso empurrou o valor para o outro lado do meio centavo. Dois defeitos se cancelando e o pior caso de correcao que existe E o **SQL** fecha o argumento do projeto: `juros.sql` e UM arquivo que roda em dois motores -- no PostgreSQL a saida e **identica ao COBOL byte a byte** (`NUMERIC` e a mesma ideia do `PICTURE`, e `ROUND` ja arredonda meio para longe de zero **por omissao**), e no SQLite **13 das 14 contas saem erradas**, porque la `NUMERIC` e *afinidade* e nao tipo: `CAST('120' AS NUMERIC)` vira `integer` e `120/100` **trunca para 1**. A correcao obvia (`/100.0`) derruba o erro para 2 de 14 -- e as duas que sobram sao justamente as contas de empate em `0,035`, que em binario **nao esta no meio** |
| P14 | **[first-code](https://github.com/Brunowdgaster/first-code)** | Curriculo de programacao visual | Pascal, Logo, Scratch, Blockly, Smalltalk | ✅ **5 de 5, 39 testes, CI 1/1.** Cinco licoes que as quatro linguagens tem de responder com o MESMO traco de tartaruga, byte a byte -- e a licao 2 prova que o laco e abreviacao com um `diff` que sai vazio, em vez da palavra do professor. O Scratch e **gerado E interpretado** no repositorio, porque .sb3 nao tem linha de comando; o Blockly tem **dois geradores**, e um emite codigo `forge` que o compilador do P02 verifica por tipo. E a licao 5 mede o limite de blocos: a arvore recursiva sai em **9 linhas de Logo e 75 blocos** de Scratch -- mesmo tendo eu CONSTRUIDO a linguagem de blocos. E o **Smalltalk ficou completo** em Pharo 11: a tartaruga e uma CLASSE, e `4 timesRepeat: [...]` e uma MENSAGEM enviada ao numero 4 -- nao sintaxe de laco. O Pharo custou quatro paredes, e tres estavam escritas na propria documentacao da ferramenta |

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

Executado até aqui: **P12 completo (11/11)**, **P14 completo (5/5)**, **P10 em
6 de 8**, **P05 em 6 de 10** e **P13 em 4 de 9**, **P04 completo (12/12)**, **P01 completo (12/12)** **P10 completo (8/8)** **P05 em 9/10** e **P02 em 6/15** — de 5 linguagens para 70. O P10 ficou parcial de propósito: faltam só T-SQL e
PL/SQL, que exigem imagens de 1,5 GB e 2,5 GB e entram por último.

### As duas ressalvas, e como foram resolvidas

| item | onde | o que era, e o que ficou |
|---|---|---|
| **VBA** | P12 | exige Microsoft Office, que é **pago** — contra a regra declarada. **Feito em LibreOffice Basic** (MPL-2.0), rotulado como dialeto diferente, sem `Option VBASupport` |
| **Smalltalk** | P14 | `gnu-smalltalk` saiu do apt, e `squeak-vm` + `etoys` instalam mas a imagem do Etoys ignora o `.st`. **Feito em Pharo 11** (MIT), instalado por download e registrado em [AMBIENTE.md](AMBIENTE.md) |

Nenhuma das duas foi declarada impossível sem ter sido tentada, e o que foi
tentado está escrito. A única linguagem do plano original que permanece sem
caminho gratuito é o **Stata**.

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

Resumo do estado em 2026-11-17: **70 linguagens distintas** em **9 projetos
publicados** de 14, todos com CI verde. **P01, P03, P04, P10, P12 e P14 estão completos.**

> O número era publicado como 45, e a auditoria do `CONTEXTO.md` achou o erro:
> `lua/registro.lua` não listava o **C#** no P13, com um comentário afirmando
> que ele "já conta em outro projeto". Não conta — o C# existe só lá. O
> portfólio vinha se **subestimando em uma linguagem**, e a correção está
> confirmada por dois instrumentos independentes: a suíte do P12 em 8/8 e a
> planilha em LibreOffice Basic, que compara o `=SUM()` do motor de cálculo
> com o contador em Lua. Mais 2 linguagens criadas do zero
(`.fasm` e `forge`).

E o nível de uso é declarado, porque 30 linguagens "usadas" não significa 30
dominadas:

| nível | quantas | o que significa |
|---|---|---|
| **implementação completa** | 12 | uma VM inteira, um verificador de tipos ou uma plataforma distribuída escrita do zero — C, Rust, Nim, Odin, D, V, Ada, Haskell, OCaml, F#, Elixir, Go |
| **ferramenta real, em produção no portfólio** | 16 | resolve um problema que o portfólio TEVE, e roda no CI — Perl, AWK, Lua, Raku, Tcl, Nix, Groovy, PowerShell, Bash, AutoHotkey, Python, SQL, PL/pgSQL, Cypher, SPARQL, GraphQL |
| **currículo verificado byte a byte** | 5 | a mesma lição nas cinco, com traço idêntico — Pascal, Logo, Scratch, Blockly, Smalltalk |
| **ferramenta real** (cont.) | 1 | LibreOffice Basic — a planilha que confere a própria conta |
| **uso genuíno, escopo menor** | 5 | Erlang, Gleam, Scala, Clojure, HCL |

A separação existe porque é o que distingue este portfólio de uma lista de
"linguagens que já toquei". Cada README declara o nível daquele projeto, e a
contagem sai de `lua/registro.lua` no p12-toolbelt, não de estimativa.

## O ambiente: ~11 GB que não estão no git

Dezenas de linguagens exigem dezenas de compiladores, e **nada disso cabe no
git** — *toolchain* não é código do projeto. O que o repositório carrega é a
instrução de como obter cada um.

**[AMBIENTE.md](AMBIENTE.md)** é o registro completo: cada pacote com a data
de instalação tirada do log do apt, a licença (todas livres), o que ele serve
em qual projeto, e os comandos para desinstalar no fim.

| onde | tamanho | o que é |
|---|---|---|
| `/nix` | **8,0 GB** | a *store* do flake do p12 — guarda uma cópia independente de tudo, que é o que a torna reproduzível e caro |
| imagens Docker | 2,0 GB | do p03 (a imagem `scratch` de 8,63 MB, kind, NATS) |
| `~/.cache` | 1,1 GB | npm, cargo, hex |
| toolchains à mão | 836 MB | V, Odin, Gleam, scala-cli — sem pacote no Ubuntu |
| pacotes apt | ~1 GB | 30 compiladores e interpretadores |

A remoção é **tarefa pendente declarada**, não esquecimento: cada projeto
precisa do seu *toolchain* para o `verifica.sh` passar, então a limpeza é a
última etapa do portfólio.

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
