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
> **Isto é uma tarefa pendente do projeto**, decidida pelo autor em
> 2026-11-17, com a condição escrita nas palavras dele: *"apagar quando
> terminar todos os projetos e eles estarem **certinhos no GitHub**"*.
>
> São **três condições**, e o `AMBIENTE.md` traz o comando que verifica cada
> uma: os 14 repositórios publicados, cada um com **CI verde na última
> execução**, e a contagem de linguagens conferida pelo `cobertura.lua`.
>
> **Não remover antes.** Cada projeto depende do seu *toolchain* para o
> `verifica.sh` passar: remover o `ghc` quebra o p02, o `postgresql` quebra o
> p10, o Pharo quebra o p14. O custo de esperar é disco, que esta máquina tem
> de sobra; o de antecipar é reinstalar um projeto inteiro para corrigir uma
> linha.
>
> Dois itens não devem ser removidos pela lista: `sqlmap` e `ffuf` foram
> instalados antes deste portfólio e não pertencem a ele.

Última atualização: 2026-11-16.

---

## 1. O objetivo, na forma em que foi pedido

> "Mostrar domínio completo e absoluto do maior número de linguagens possíveis."

Catorze projetos de engenharia que, somados, visam **100 linguagens** — a
meta declarada em `registro.lua`, que é a que vale. Cada linguagem aparece
**onde é a escolha tecnicamente certa**, com resultado medido e trade-off
escrito — não como pasta com *hello world*.

### A calibração que o portfólio assume

Ninguém domina 100 linguagens de forma absoluta. O que estes repositórios
demonstram, e declaram em cada README:

- **profundidade real** em um núcleo de linguagens (hoje **63**, contadas por
  ferramenta e não à mão — ver a seção 2);
- **fluência de leitura, porte e avaliação de trade-off** nas demais.

Essa distinção é deliberada e é parte do valor: um portfólio que mente sobre
profundidade é descoberto na primeira pergunta técnica.

---

## 2. Onde está, medido em 2026-11-16

**63 linguagens distintas, de uma meta declarada de 100.** O número vem da
ferramenta, não da mão:

    lua5.4 lua/cobertura.lua lua/registro.lua --roadmap ../poliglota/ROADMAP.md

Rodada em `p12-toolbelt/`. A soma por projeto dá 64; as distintas são 63
porque **Rust é a única sobreposição** (P01 e P03), e a ferramenta desconta.

> ### O que a auditoria desta data corrigiu
>
> Este arquivo dizia **"5 de ~130 — 3,8 %"**, medido em 2026-11-14, enquanto a
> tabela três parágrafos abaixo já listava 23 linguagens e o `README.md` dizia
> 45. Era o índice defasado — o mesmo defeito que o `lint_medicoes.pl` existe
> para achar, e que a seção 6 catalogou dez vezes.
>
> E a conta publicada em si estava errada: o `registro.lua` **não listava o
> C#**, com um comentário afirmando que ele "já conta em outro projeto". Não
> conta — o C# existe só no P13. O `README.md` do P13 sempre disse "3 das 9";
> era o registro que estava errado, e o portfólio vinha se **subestimando** em
> uma linguagem. Corrigido: **45 → 46**, com a suíte do P12 em 8/8.

### Três números diferentes, e o que cada um significa

| número | de onde vem | o que é |
|---|---|---|
| **121** | soma de `itens_no_plano` no ROADMAP | itens do plano, incluindo o que **não é linguagem** (Makefile, CMake, YAML, Dockerfile) |
| **100** | `meta` em `registro.lua` | a meta de linguagens de fato — é contra esta que se mede |
| ~130 | prosa original do ROADMAP | número de divulgação, nunca foi contado. **Não use** |

**63 de 100 — 63 %. Faltam 37.**

### As 63, com a evidência ao lado

| # | linguagem | projeto | evidência |
|---|---|---|---|
| 1 | C | P01 | VM + GC + montador, gcc/clang/MSVC |
| 2 | Rust | P01, P03 | VM completa sem `unsafe`; NIF medindo ~30 ns de travessia |
| 3 | Nim | P01 | porte da VM em **255 linhas**, 9/9, ORC do runtime |
| 4 | Odin | P01 | porte da VM, 9/9, `virtual.Arena` e `context.allocator` |
| 5 | D | P01 | porte da VM, 9/9, `SumType` exaustivo |
| 6 | V | P01 | porte da VM, 9/9, handle+arena imposto pelo tipo-soma |
| 7 | Ada | P01 | porte da VM, 9/9, subtipos restritos e `Unchecked_Conversion` |
| 8 | Haskell | P02 | compilador com inferência de Hindley-Milner completa |
| 9 | OCaml | P02 | porte do verificador, **23/23 byte a byte**, 604 linhas |
| 10 | F# | P02 | porte do verificador, 23/23, **496 linhas** — menos que o Haskell |
| 11 | Elixir | P03 | núcleo OTP, 700 mil conexões, 35 testes |
| 12 | Erlang | P03 | conexão alternativa na mesma árvore; **1 byte** de diferença |
| 13 | Gleam | P03 | codec tipado, 13 testes, `@external` para OTP sem FFI |
| 14 | Go | P03 | gateway WebSocket sem dependências, 7,1 KB/conexão |
| 15 | Scala | P03 | Loom contra BEAM: 1.323 vs 2.732 bytes, inanição medida |
| 16 | Clojure | P03 | atom/ref/agent contra GenServer; 2 patologias achadas |
| 17 | HCL | P03 | OpenTofu, 20 asserções verificando 5 propriedades |
| 18 | Fortran | P05 | `modelo.f90`: os **dois** algoritmos, à mão, no mesmo arquivo |
| 19 | R | P05 | `modelo.R`: a fórmula como objeto de primeira classe |
| 20 | Octave | P05 | `modelo.m`: álgebra linear como sintaxe (dialeto MATLAB) |
| 21 | hansl | P05 | `modelo.inp`: no lugar do Stata, que é pago |
| 22 | Python | P10 | `referencia.py`: o BFS sem magia que as outras 7 conferem |
| 23 | SQL | P10 | `alcance.sql`: o **mesmo arquivo** em SQLite e PostgreSQL |
| 24 | PL/pgSQL | P10 | `transferir.sql`: atomicidade, e a classe QL de `ERRCODE` |
| 25 | Cypher | P10 | `alcance.cypher`: travessia como primitiva, em Kuzu |
| 26 | SPARQL | P10 | `alcance.rq`: o padrão **não** limita repetição, e a inferência |
| 27 | GraphQL | P10 | `esquema.graphql`: o que o modelo **não** expressa, medido |
| 28 | Perl | P12 | `lint_medicoes.pl`: número publicado sem dispersão ao lado |
| 29 | AWK | P12 | `estatistica.awk`: faixa, dispersão e a regra de decisão |
| 30 | Lua | P12 | `cobertura.lua`: esta própria contagem |
| 31 | Raku | P12 | `gramatica_fasm.raku`: **9/9 byte a byte** contra o desmontador em C |
| 32 | Tcl | P12 | `dirige_repl.exp`: 12 interações em dois REPLs, com Expect |
| 33 | Nix | P12 | `flake.nix`: 4 devShells; achou um bug que só um 2º ambiente acha |
| 34 | Groovy | P12 | `relatorio.groovy`: saída dos verificadores em JUnit XML |
| 35 | PowerShell | P12 | `Audita-Repos.ps1`, com par em Bash: as duas concordam |
| 36 | Bash | P12 | `bash/audita-repos.sh`: o par POSIX do PowerShell |
| 37 | AutoHotkey | P12 | `Verifica-Ambiente.ahk`: nomeia a causa que o `calibra.sh` só sente |
| 38 | LibreOffice Basic | P12 | `Cobertura.bas`: a cobertura como planilha, com `=SUM()` de verdade |
| 39 | COBOL | P13 | `juros.cob`: o lote original, e a especificação executável |
| 40 | C# | P13 | `Juros.cs`: com `decimal`, idêntico ao COBOL **byte a byte** |
| 41 | VB.NET | P13 | `Juros.vb`: a ponte que as empresas de fato usaram |
| 42 | Pascal | P14 | `licoes.pas`: a referência, com aviso tratado como erro |
| 43 | Logo | P14 | `licoes.lg`: `openwrite`/`setwrite` porque o build é só gráfico |
| 44 | Scratch | P14 | `gera.py` + `simula.py`: `.sb3` gerado **e** interpretado aqui |
| 45 | Blockly | P14 | `blocos.js`: dois geradores, um deles emite código `forge` |
| 46 | Smalltalk | P14 | `licoes.st`: classe de verdade, em Pharo 11 headless |
| 47 | Object Pascal | P13 | `juros.pas`: `Currency` bate byte a byte, e o `--double` concordava **por acidente** |
| 48 | JavaScript | P04 | `js/view.js`: a referência, sem transpilação |
| 49 | TypeScript | P04 | `ts/view.ts`: e 4 bugs que o `tsc` **tem de recusar**, com os códigos medidos |
| 50 | CoffeeScript | P04 | `view.coffee`: o arquivo arqueológico — e ele **ainda compila** |
| 51 | PHP | P04 | `view.php`: o legado, e o `htmlspecialchars` que escreve `&#039;` |
| 52 | Ruby | P04 | `view.rb`: o painel, e o `CGI.escapeHTML` que concorda **por acidente** |
| 53 | HTML | P04 | `dados/esperado/view.html`: o alvo que as cinco acertam byte a byte |
| 54 | CSS | P04 | `css/orcamento.css`: artefato gerado, commitado e verificado |
| 55 | Sass | P04 | `orcamento.scss`: e a decisão honesta de que **não se paga** nesta folha |
| 56 | Elm | P04 | `View.elm`: MVU e *ports* — e o Elm precisou de um harness em JS, que é **o preço da garantia** |
| 57 | PureScript | P04 | `Main.purs`: classes de tipo, e o FFI que **compilou e quebrou em execução** |
| 58 | Dart | P04 | `view.dart`: `sound null safety`, e quatro alvos de compilação |
| 59 | C++ | P01 | `fvm.cpp`: `shared_ptr` **não coleta ciclo** — e o ORC do Nim coleta |
| 60 | Zig | P01 | `fvm.zig`: alocador na assinatura, e a tabela de operandos derivada do enum em `comptime` |
| 61 | Assembly x86-64 | P01 | `ports/asm/x86_64/fvm.s`: sem libc, `_start` e syscalls — o **subconjunto** do despacho |
| 62 | Assembly ARM64 | P01 | `ports/asm/aarch64/fvm.s`: `cset` em vez de `setcc`+`movzx`, e não existe `open` |
| 63 | Assembly RISC-V | P01 | `ports/asm/riscv64/fvm.s`: `slt` em **uma** instrução, e nenhum registrador de condição |

### A convenção de contagem mudou — e por quê

A versão anterior deste arquivo tinha uma categoria **"auxiliares genuinamente
usadas, que não contam"**: Bash, YAML, PowerShell, CMake. Ela está obsoleta
para três delas, e a regra que a substitui está escrita em `registro.lua`:

> Uma linguagem conta quando tem, naquele projeto: código que faz trabalho real
> (não hello-world); verificação automatizada que falha se o código quebrar; e
> uma seção de README explicando o que a linguagem deu ou cobrou. **Porte
> declarado como porte conta. Script de build não conta** — é ferramenta, não
> produto.

Bash, PowerShell e Nix passaram a contar porque no P12 deixaram de ser cola de
build e **viraram o produto**: o par `Audita-Repos.ps1` / `audita-repos.sh`
audita os repositórios nas duas direções e as duas saídas têm de concordar; o
`flake.nix` define 4 devShells e achou um bug que só um segundo ambiente acha.
YAML e CMake continuam **não** contando — são configuração.

### Criadas do zero: 2

`.fasm` (390 linhas em 12 programas) e `forge` (132 linhas em 24 programas) —
projetadas **e** implementadas, com gramática, montador e compilador.

### Progresso contra o plano

| projeto | planejadas | feitas | o que falta |
|---|---|---|---|
| P01 metal-vm | 12 | **12 — completo** | — |
| P02 forge-lang | 15 | **3** — 20 % | Lisps ×3, Prolog, Lean, Coq, Agda, Idris, Isabelle, TLA+, WAT |
| P03 otp-mesh | 8 | **8 — completo** | — |
| P04 webstack | 10 | 0 | tudo |
| P05 datalab | 10 | **4** — 40 % | Julia (fora do apt), Scala/Spark, Mojo, Wolfram, SAS |
| P06 gpu-arena | 8 | 0 | tudo |
| P07 engine | 6 | 0 | tudo |
| P08 tri-mobile | 5 | 0 | tudo |
| P09 silicon | 8 | 0 | tudo |
| P10 query-lab | 8 | **6** — 75 % | T-SQL e PL/SQL (imagens de 1,5 e 2,5 GB) |
| P11 chain | 6 | 0 | tudo |
| P12 toolbelt | 11 | **11 — completo** | — |
| P13 legacy | 9 | **4** — 44 % | PL/I, RPG, ABAP, Apex, SQL |
| P14 first-code | 5 | **5 — completo** | — |

> Atenção à diferença entre as duas contagens, que **não** é erro: o P05 feito
> são 4 aqui e "6 de 10" no seu próprio README, porque Python e SQL já contam
> no P10. Por projeto, cada linguagem conta; no total, uma vez só. O
> `cobertura.lua` é quem resolve isso, e por isso a conta não deve ser refeita
> à mão.

### Bloqueios conhecidos, com a causa

| item | onde | o que bloqueia |
|---|---|---|
| T-SQL, PL/SQL | P10 | imagens de 1,5 GB e 2,5 GB; a suíte já sabe pular, e `QLAB_EXIGE=1` recusa pular no CI |
| RPG, ABAP, Apex | P13 | exigem **conta gratuita de terceiro** (PUB400, SAP BTP, Salesforce) — decisão pendente do autor |
| Julia | P05 | não está no apt desta distribuição |
| Stata | P05 | **sem caminho gratuito** — a única do plano original nessa situação |
| VBA | P12 | exige Office pago; **resolvido** com LibreOffice Basic, rotulado como outro dialeto |

---

## 3. Repositórios publicados

Nove repositórios públicos, licença MIT, CI verde, na conta `Brunowdgaster`.
Todos limpos e sincronizados com o remoto em 2026-11-16 (verificado com
`git status` e `git rev-list --left-right --count HEAD...@{u}`).

| repo | o que é | verificação | linguagens |
|---|---|---|---|
| [poliglota](https://github.com/Brunowdgaster/poliglota) | índice: roadmap, viabilidade, ambiente, este arquivo | — | — |
| [metal-vm](https://github.com/Brunowdgaster/metal-vm) | VM de bytecode, 2 referências + 5 portes | 9 casos, 54/54 cross-compat, CI 10/10 | 7 |
| [forge-lang](https://github.com/Brunowdgaster/forge-lang) | linguagem funcional + compilador + 2 portes do verificador | 39/39, 23/23 byte a byte nos portes | 3 |
| [otp-mesh](https://github.com/Brunowdgaster/otp-mesh) | plataforma OTP + gateway Go — **completo** | 10/10 + 7/7, CI 9/9 | 8 |
| [datalab](https://github.com/Brunowdgaster/datalab) | mínimos quadrados em 6 linguagens, com tolerância justificada | 11 verificações | 6 (4 novas) |
| [query-lab](https://github.com/Brunowdgaster/query-lab) | a mesma pergunta em 6 dialetos, 3 modelos de dados | 22 testes | 6 |
| [toolbelt](https://github.com/Brunowdgaster/toolbelt) | ferramentas do próprio portfólio — **completo** | 49 testes POSIX + 9 Windows | 11 |
| [legacy-bridge](https://github.com/Brunowdgaster/legacy-bridge) | lote COBOL migrado para C# e VB.NET, byte a byte | 16 verificações | 3 |
| [first-code](https://github.com/Brunowdgaster/first-code) | o mesmo currículo em 5 linguagens visuais — **completo** | 39 testes | 5 |

Pastas locais em `C:\Users\bruno\Desktop\Projetos\para se gabar\`. As
catorze existem; **`p04`, `p06`, `p07`, `p08`, `p09` e `p11` são placeholders**
— só `docs/` e `src/` vazios, sem git.

---

## 4. A ordem de trabalho decidida

A ordem foi cortada **por camada de custo, atravessando os projetos**, e não
projeto por projeto em profundidade. Isso veio de uma correção do autor,
registrada na **[seção 9](#9-a-correção-que-mudou-a-ordem-de-trabalho)**.

### O que já foi executado nessa ordem

**P03 completo** (8/8), **camada 1 do P01** (5 portes da VM), e depois
**P12 → P10 → P14 → P05 → P13**. De 5 linguagens para **46**. O P12 e o P14
fecharam 100 %; o P10 ficou em 6 de 8 de propósito, porque o que falta são
duas imagens de contêiner de vários gigabytes.

### Os próximos, por custo crescente

> **Feito em 2026-11-16:** o item 1 desta lista — **Object Pascal no P13** —
> está pronto e publicado. O P13 foi de 3 para 4 de 9, com 22 verificações, e
> rendeu o achado da **precisão simples do FPC** descrito na seção 6. A lista
> abaixo já reflete isso.

> **Feito em 2026-11-16:** o item 1 — **P04 webstack** — está publicado com 9
> de 12 e 21 verificações. Faltam Elm, PureScript e Dart, que são as três vias
> que o entregável do plano exige para a tabela de *bundle size*.

> **Feito em 2026-11-16:** o **P04 está completo (12/12, 29 verificações)**.
> Elm, PureScript e Dart entraram, e as três produzem HTML byte a byte
> idêntico. A tabela de *bundle size* que o plano promete **não** entrou: ver
> a nota no README do projeto sobre por que medir sem método não serve.

> **Feito em 2026-11-17:** o **P01 foi de 7 para 10 de 12** — C++, Zig e
> Assembly x86-64, com os sete portes em 9/9 e o assembly em 4/4. Faltam
> Assembly ARM64 e RISC-V: as *toolchains* de cross e o `qemu-user` **já estão
> instalados**, e só o `.s` de cada arquitetura não existe. A suíte os marca
> como pulados em vez de fingir que passam.

> **Feito em 2026-11-17:** o **P01 está completo em linguagens (12/12)**. Os
> três portes em Assembly passam 12/12 nas três arquiteturas, e o ARM64 e o
> RISC-V passaram na **primeira montagem** — porque o mapa de registradores
> foi escrito antes do código, que é a lição que o segfault do x86-64 deixou.
>
> Faltam no P01 só os três itens de **build** do plano (Makefile, CMake, Nix),
> que não são linguagens de programação.

| ordem | o que | linguagens | por quê agora |
|---|---|---|---|
| 1 | **P10**: T-SQL e PL/SQL | +2 | fecham o query-lab; exigem contêineres de 1,5 GB e 2,5 GB, e o `docker` já está instalado |
| 2 | **P13**: SQL, PL/I | +1 | o SQL não acrescenta distinta (já conta no P10) mas fecha mais um; o PL/I precisa do Iron Spring |
| 3 | **P13**: SQL | +0 | a mesma regra como consulta; não acrescenta linguagem distinta (SQL já conta no P10), mas fecha o P13 em 5 de 9 e o `sqlite3` já está instalado |
| 4 | **P06** gpu-arena | +8 | CUDA funciona no WSL2, mas **Triton exige compute capability ≥ 7.0** e a MX150 é 6.1 → Colab |
| 5 | P07, P08, P09, P11 | ~25 | produtos maiores; o P08 depende de macOS para uma das vias |
| 6 | **P02** restante | +12 | provas em Lean/Coq/Agda/Idris: o trabalho mais caro de todos, e o de maior valor técnico |

O **PL/I, RPG, ABAP e Apex** do P13 ficam fora desta ordem de propósito:
nenhum compilador de PL/I está instalado (`plic`/`pli` ausentes, verificado em
2026-11-16), e os outros três exigem conta gratuita em serviço externo
(PUB400, SAP BTP, Salesforce). Eles entram como módulos documentados, que é o
que o plano original pede.

Duas decisões do autor estão pendentes e não devem ser tomadas sem ele:
**abrir ou não as contas gratuitas** de PUB400, SAP BTP e Salesforce (vale RPG,
ABAP e Apex, +3) e **baixar ou não** as imagens de T-SQL e PL/SQL (+2, 4 GB).

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

## 6. A lição que se repetiu onze vezes

**O instrumento esconde o próprio sinal.** Começou como "falha silenciosa:
redirecionar a saída e não olhar o código de retorno", e as ocorrências
seguintes mostraram que o padrão é mais largo que isso: qualquer instrumento
que possa dizer "ok" sem ter verificado.

| # | onde | como escondeu |
|---|---|---|
| 1 | `bench/run_bench.sh` (P01) | pulava binários inexistentes em silêncio |
| 2 | `Mesh.Chaos.kill_fraction` (P03) | devolvia contagem parcial no timeout |
| 3 | `setup.sh` (P03) | `apt-get install` sem `update` e sem checar retorno |
| 4 | `tests/smoke.sh` (P03) | `2>/dev/null` escondia o motivo da falha |
| 5 | `cmd/loadgen` (P03) | tratado **preventivamente**: sai com erro se abrir menos conexões do que pediram |
| 6 | `bench/nif.exs` (P03) | media A em bloco e B em bloco; duas execuções deram **conclusões opostas** |
| 7 | `setup-opentofu` no CI (P03) | o wrapper da action **engolia o exit code** de `plan -detailed-exitcode`: passava local, falhava no CI, e reportava "nenhum desvio" em vez de "não sei" |
| 8 | `pwsh/Audita-Repos.ps1` (P12) | `$?` diz se o PowerShell conseguiu **invocar** o programa, não o que o programa devolveu — o código real está em `$LASTEXITCODE`, e a diferença é **silenciosa**. Em Bash as três camadas colapsam em um `if` |
| 9 | `perl/lint_medicoes.pl` (P12) | três falsos negativos **no próprio lint**: o contexto do parágrafo atravessava o cabeçalho da seção seguinte (um título com a palavra "dispersão" silenciava o achado); faltava `use utf8`, então `/µs/` eram **dois bytes** contra **um caractere** na entrada; e a saída saía em mojibake. Achados por **testar as duas direções**, não por leitura |
| 10 | `registro.lua` (P12) | o comentário dizia que o C# "já conta em outro projeto" e ele só existe no P13: a ferramenta que existe para a conta **publicou 45 em vez de 46** desde que o P13 entrou no registro. Achado pela auditoria deste arquivo em 2026-11-16 |
| 11 | `pascal/juros.pas` (P13) | **dois defeitos se cancelaram, e o teste passou.** O modo `--double` existe para DIVERGIR do COBOL, e ele concordava. A causa: o FPC avalia `Integer / <literal real>` em precisão **simples** — erro relativo de `4,7e-08`, cinco ordens pior que `Double` — e isso empurrou o valor para o outro lado do meio centavo. Achado por uma sonda (`pascal/sonda_precisao.pas`) escrita só para medir por que o teste passava |

A décima primeira é de uma espécie nova e pior: as dez anteriores eram
instrumentos que diziam "ok" sem verificar. Esta é um **programa errado por
dois motivos ao mesmo tempo, com os dois erros se anulando no resultado**.

> Quando um teste passa por acidente, ele deixa de ser teste e passa a ser
> ruído com aparência de evidência. E a única forma de descobrir é desconfiar
> de um teste que passa quando você esperava que falhasse.

**Conhecer o modo de falha não imuniza contra ele** — onze vezes é prova
disso.
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
- **Um teste sobre CÓDIGO que lê também a PROSA** — quatro vezes, todas no
  mesmo dia. Um `grep` que proíbe uma construção acusa o comentário que
  explica por que ela é proibida: `Option VBASupport` (P12), `methodsFor:`
  (P14), o separador de chunk (P14) e `Taxa / 10000.0` (P13). A correção é
  sempre a mesma — uma passagem que rastreia o estado de comentário da
  linguagem — e o padrão é geral: **`grep` não sabe onde o código termina.**

- **O caso espelhado, que é outro bug e não o mesmo**: no P05, o teste "a
  referência gravada está em dia?" comparava o JSON do instantâneo **como
  texto**, passava aqui e **falhava no CI** — porque o LAPACK do runner não é o
  mesmo build e a SVD muda os últimos bits. Não é falso OK, é **falso alarme**;
  e a causa é irônica: a disciplina de tolerância que o projeto inteiro pregava
  não se aplicava ao instrumento do próprio projeto. Hoje o instantâneo também
  é comparado com `1e-10`, e o teste separa as duas causas possíveis.

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

---

## 9. A correção que mudou a ordem de trabalho

A seção 4 referencia esta seção, e até 2026-11-16 ela **não existia no
arquivo** — a referência apontava para o vazio. O conteúdo estava só na memória
da sessão, que é exatamente o lugar errado para ele. Está aqui agora.

O autor me corrigiu **duas vezes** na mesma direção: o objetivo é **domínio
demonstrado do maior número de linguagens possível**, com os projetos
concluídos — e não profundidade máxima por linguagem.

O `ROADMAP.md` sempre teve **dois níveis**, e eu só executava o primeiro:

1. **implementação de referência** — funda, medida, com ensaio próprio;
2. **porte do núcleo mínimo** (~300 linhas), declarado no README como porte e
   **não** como produção.

Tratar todo porte como implementação de referência fez o P01 parar em 2 de 12
linguagens e o P02 em 1 de 15, enquanto o P03 — horizontal por natureza —
fechou 8 de 8. O critério de sucesso é **cobertura com evidência honesta**, não
profundidade máxima por linguagem.

### Como aplicar, na prática

- Cortar o trabalho **por camada de custo, atravessando os projetos**, e não
  projeto por projeto em profundidade.
- Para um porte, **"feito" = passa na suíte que já existe + seção de README
  dizendo que é porte**. Não refazer medição, não inventar ensaio novo: os 54
  testes do P01 e os 39 do P02 **são** a especificação.
- A ressalva, que o autor aceitou: **porte só tem valor ao lado de uma
  implementação de referência.** Por isso esta ordem só passou a valer depois
  que as três âncoras (P01, P02, P03) existiam. Ela não é licença para encher o
  portfólio de portes sem fundo.

---

## 10. Os projetos fechados depois do P03 — não refazer

Mesma regra da seção 8: o que está aqui está medido e verificado. Os números
vêm dos READMEs de cada repositório, que são a fonte.

### P12 toolbelt — 11/11, 49 testes POSIX + 9 Windows

Ferramentas que resolvem problemas que **este portfólio teve**, e três delas
acharam erros meus na primeira execução:

- `perl/lint_medicoes.pl` achou **duas linhas defasadas** neste próprio
  arquivo: carregavam 1.400× (ETS) e 254× (`off_heap`), números
  **pré-auditoria**, quando o README do P03 já dizia ~912× e ~66×. Eu corrigi o
  README e esqueci o índice;
- `lua/cobertura.lua` corrigiu a contagem de linguagens na primeira execução —
  eu dizia 17 e eram 20, porque não contava as ferramentas que acabara de
  escrever. E em 2026-11-16 a auditoria achou o inverso: a ferramenta estava
  **subestimando** em 1, por um comentário errado no `registro.lua`;
- `pwsh/Audita-Repos.ps1` contra `bash/audita-repos.sh`: as duas têm de
  concordar, e foi o par que revelou as três camadas de código de retorno do
  PowerShell (seção 6, linha 8);
- `raku/gramatica_fasm.raku`: **9/9 byte a byte** contra o desmontador em C;
- `nix/flake.nix`: 4 devShells, e achou um bug que só um segundo ambiente acha.

### P14 first-code — 5/5, 39 testes

Cinco lições, cinco linguagens, **o mesmo traço de tartaruga byte a byte**
(`F 100`, `R 90`, `PU`) contra uma referência — a prova é um `diff`, não uma
captura de tela. O formato está em `TRACO.md`.

O Smalltalk exigiu **Pharo 11 por download**: `gnu-smalltalk` saiu do apt, e
`squeak-vm` + `etoys` instalam mas a imagem do Etoys ignora o `.st`. Pharo é
MIT e autocontido, registrado em `AMBIENTE.md` com o comando de remoção.

### P10 query-lab — 6/8, 22 testes

A mesma pergunta — *quem está a até 3 saltos de amizade de Ana?* — em 6
dialetos e **3 modelos de dados**, todos obrigados a dar a mesma resposta. A
tese não é "sei oito dialetos", é que **modelo de dados é uma escolha com
consequências**. O `alcance.sql` é o **mesmo arquivo** rodando em SQLite e
PostgreSQL; o SPARQL mostrou que o padrão **não limita repetição**; o
`esquema.graphql` mede o que o modelo **não** expressa.

Parcial de propósito: T-SQL e PL/SQL exigem imagens de 1,5 GB e 2,5 GB. A
suíte já sabe pular, e `QLAB_EXIGE=1` **recusa pular** no CI.

### P05 datalab — 6/10 (4 novas), 11 verificações

Mínimos quadrados em 6 linguagens — 7 implementações, porque o Fortran traz os
**dois algoritmos à mão** no mesmo arquivo — comparadas com **tolerância
justificada** em vez de `diff`. A tolerância está escrita e defendida em
`TOLERANCIA.md`.

Dois detalhes que valem lembrar: a suíte **exige que algumas implementações
divirjam** no caso mal-condicionado (se passassem a concordar, a tese do
projeto estaria errada, e é esse teste que avisaria); e o instantâneo gravado
também é comparado com tolerância — ver o caso espelhado na seção 6.

### P13 legacy-bridge — 3/9, 16 verificações

Um lote bancário em COBOL migrado para C# e VB.NET, com equivalência provada
por `cmp`. Com `decimal`, as duas são **idênticas ao COBOL byte a byte**; com
`double`, a **mesma regra, mesma ordem de operações, mesmo arredondamento**
perde **um centavo** na conta 13.

As contas 13 e 14 **não foram escolhidas à mão** — está dito em `dados/gera.py`
que um programa varreu saldos de 0,01 a 2.000,00 contra taxas de 1 a 2.500
emulando os dois caminhos, porque as 12 contas escolhidas à mão saíram todas
idênticas. A previsão não se confirmou de primeira, e a busca é o que a
sustenta.

E a armadilha de ambiente, que custou três tentativas com a mesma mensagem
`BC30420: 'Sub Main' was not found`: **o VB.NET prepende o `RootNamespace` do
projeto ao namespace declarado no arquivo**, então o tipo real era
`LegacyBridge.LegacyBridge.Juros`. No C# ao lado o namespace do arquivo é
**absoluto** — duas linguagens da mesma plataforma com regra oposta para a
mesma propriedade, e nada no erro aponta para isso.
