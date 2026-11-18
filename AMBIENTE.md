# Ambiente: o que foi instalado, por quê, e como desinstalar

Este portfólio cobre dezenas de linguagens, e cada uma exige um compilador, um
interpretador ou um motor. **Nada disso cabe no git** — binários e *toolchains*
não são código do projeto, são o ambiente em que ele roda.

Então este arquivo é o registro. Ele existe por três motivos:

1. **para desinstalar depois.** O portfólio ocupa cerca de **11 GB** em
   *toolchains*, e a maior parte não serve para mais nada quando o projeto
   acabar. A seção [Desinstalando](#desinstalando) tem os comandos;
2. **para reinstalar.** Um clone novo numa máquina limpa precisa saber o que
   instalar, e cada README diz o que o seu projeto exige — este arquivo é a
   soma;
3. **para a conta de custo ser verificável.** A regra declarada do portfólio é
   **nenhum recurso pago**, e a coluna "licença" abaixo é o que torna isso
   conferível em vez de afirmado.

> **Tudo nesta página é livre e gratuito.** Nenhum item exige conta paga,
> licença comercial, teste expirável ou cartão de crédito. Os dois itens que
> **exigiriam** pagamento estão em [O que foi recusado por ser
> pago](#o-que-foi-recusado-por-ser-pago), e não foram instalados.

Tudo roda dentro do **WSL2 (Ubuntu 26.04)**, exceto os dois itens marcados
como Windows.

## Resumo do espaço

| onde | tamanho | o que é |
|---|---|---|
| `/nix` | **8,0 GB** | a *store* do Nix, do flake do p12-toolbelt |
| `~/toolchains` | 478 MB | V e Odin, baixados à mão (não há pacote apt) |
| `/usr/local/bin` | 358 MB | Gleam e scala-cli, instalados por tarball |
| imagens Docker | 2,0 GB + **16,0 GB** | do p03 (Go em `scratch`, kind, NATS), mais SQL Server (2,21 GB) e Oracle (13,8 GB) do p10 |
| `~/.cache` | 1,1 GB | caches de npm, cargo, hex, stack |
| `~/pharo` | 310 MB | VM e imagem do Pharo, do p14-first-code |
| LibreOffice | ~400 MB | o Basic do p12-toolbelt |
| pacotes apt | ~1,4 GB | ver a tabela abaixo |

O `/nix` é de longe o maior, e é o primeiro a remover: ele guarda uma cópia
independente de **todas** as dependências do flake, de propósito — é o que faz
o ambiente ser reproduzível, e o que o torna caro.

## Pacotes do apt

Instalados com `apt-get install`, e removíveis com `apt-get purge`. A data é
do log do apt (`/var/log/apt/history.log`), não de memória.

| pacote | data | para quê | licença |
|---|---|---|---|
| `build-essential`, `cmake` | 2026-11-13 | C e C++ do p01-metal-vm | GPL |
| `rustc`, `cargo`, `rustfmt`, `rust-clippy` | 2026-11-13 | Rust do p01 e o NIF do p03 | MIT/Apache-2.0 |
| `linux-tools-common`, `linux-tools-generic` | 2026-11-14 | `perf`, para medir o p01 | GPL |
| `ghc` | 2026-11-14 | Haskell: o compilador do p02-forge-lang | BSD-3 |
| `erlang`, `elixir`, `rebar3` | 2026-11-14 | a plataforma do p03-otp-mesh | Apache-2.0 |
| `golang-go` | 2026-11-14 | o gateway do p03 | BSD-3 |
| `openjdk-21-jdk` | 2026-11-14 | Scala e Groovy, e o Loom do p03 | GPL+CE |
| `clojure` | 2026-11-14 | o `agent` vs GenServer do p03 | EPL-1.0 |
| `docker.io` | 2026-11-14 | a imagem `scratch` de 8,63 MB do p03 | Apache-2.0 |
| `nim`, `gnat`, `ldc`, `dub` | 2026-11-14 | portes Nim, Ada e D do p01 | variados, livres |
| `clang` | 2026-11-14 | o Odin precisa dele como *linker* | NCSA |
| `ocaml` | 2026-11-14 | o porte OCaml do p02 | LGPL-2.1 |
| `dotnet-sdk-10.0` | 2026-11-14 | o porte F# do p02 | MIT |
| `lua5.4` | 2026-11-14 | `cobertura.lua` do p12 | MIT |
| `tcl`, `expect` | 2026-11-14 | `dirige_repl.exp` do p12 | BSD |
| `rakudo` | 2026-11-14 | `gramatica_fasm.raku` do p12 | Artistic-2.0 |
| `groovy` | 2026-11-14 | `relatorio.groovy` do p12 | Apache-2.0 |
| `nix-bin` | 2026-11-14 | o flake do p12 | LGPL-2.1 |
| `sqlite3` | 2026-11-15 | SQL ANSI do p10-query-lab | domínio público |
| `postgresql`, `postgresql-client` | 2026-11-15 | PL/pgSQL do p10 | PostgreSQL License |
| `python3-pip`, `python3-venv` | 2026-11-15 | o venv dos motores do p10 | PSF |
| `fp-compiler` | 2026-11-15 | Pascal do p14-first-code | GPL |
| `ucblogo` | 2026-11-15 | Logo do p14 | GPL |
| `xvfb` | 2026-11-15 | **necessário** para o Logo — ver abaixo | MIT |
| `nodejs`, `npm` | 2026-11-15 | Blockly do p14 | MIT |
| `squeak-vm`, `etoys` | 2026-11-15 | tentativa de Smalltalk que **não funcionou** | MIT |
| `libreoffice-core`, `libreoffice-calc`, `libreoffice-script-provider-python` | 2026-11-15 | **LibreOffice Basic** do p12 — ver abaixo | MPL-2.0 |
| `unzip` | 2026-11-14 | extrair o V | Info-ZIP |
| `sqlmap`, `ffuf` | anteriores | **não são deste portfólio** | — |

### `xvfb` merece explicação

O pacote `ucblogo` do Ubuntu é compilado contra wxWidgets/GTK3/X11: é build
**só gráfico**. Sem um display ele não escreve nada em stdout — nem `--help` —
e sai com código 0.

`xvfb` dá um display virtual, e aí o UCBLogo real executa o programa real. A
saída é obtida pelo `openwrite`/`setwrite` da própria linguagem Logo, que
escreve num arquivo. Está detalhado no
[README do p14](https://github.com/Brunowdgaster/first-code).

### LibreOffice: instalado por decisão explícita

Foram **49 pacotes**, instalados com `--no-install-recommends` para não trazer
a interface gráfica inteira. O que eles habilitam é
`p12-toolbelt/basic/Cobertura.bas`: uma macro em **LibreOffice Basic** que
monta a cobertura de linguagens do portfólio como planilha auditável, com
`=SUM()` de verdade em vez de um total pré-somado.

**É livre (MPL-2.0)**, e é o substituto declarado do VBA — que exigiria Office
pago. A macro **não** usa `Option VBASupport`, de propósito: o objetivo é
mostrar a API nativa (UNO), não escrever VBA fingido num produto que não é o
Office. A suíte do p12 tem um teste que falha se esse `Option` aparecer.

### `squeak-vm` e `etoys` podem ser removidos agora

Foram instalados para tentar rodar Smalltalk sem interface. **Não funcionou**:
a VM sobe (`squeakvm -version` responde) mas a imagem do Etoys ignora o
arquivo `.st` passado como argumento, e nada é avaliado. Ficam registrados
aqui porque estão instalados, não porque sejam usados.

## Instalados fora do apt

Estes não têm pacote no Ubuntu 26.04, e foram baixados dos projetos oficiais.

| o que | onde | tamanho | para quê | licença |
|---|---|---|---|---|
| **V** | `~/toolchains/vlang` | parte dos 478 MB | porte V do p01 | MIT |
| **Odin** | `~/toolchains/odin` | parte dos 478 MB | porte Odin do p01 | BSD-3 |
| **Gleam** | `/usr/local/bin/gleam` | parte dos 358 MB | o nó Gleam do p03 | Apache-2.0 |
| **scala-cli** | `/usr/local/bin/scala-cli` | parte dos 358 MB | o gerador de carga do p03 | Apache-2.0 |
| **Nix store** | `/nix` | **8,0 GB** | o flake do p12 | LGPL-2.1 |
| **Pharo 11** | `~/pharo` | 310 MB | **Smalltalk** do p14 | MIT |

### Mojo: pelo `pixi`, e SEM conta — o que mudou desde os tutoriais

**`~/ferramentas/mojo` 899 MB + `~/.cache/rattler` 103 MB + `~/.pixi` 70 MB =
~1,07 GB**, Mojo 1.2.0 — para o `p05-datalab`.

Licença: a biblioteca padrão é **Apache-2.0 com exceção de LLVM**; o
compilador é proprietário e **gratuito**, inclusive para uso comercial.
Nenhum pagamento, e — o ponto que importa — **nenhuma conta**.

Houve época em que a Modular exigia `modular auth` com um token do site. Isso
acabou, e foi **medido antes de instalar**:

```
https://conda.modular.com/max/linux-64/repodata.json   ->  HTTP 200, sem token
pacotes no canal: ['max', 'mojo', 'mojo-compiler']
```

Instalado com `pixi` (0.84.2, instalado junto), num projeto dedicado em vez de
global, para que a remoção seja um `rm -rf` e não uma caça a pacotes.

Remoção:

```bash
rm -rf ~/ferramentas/mojo ~/.cache/rattler ~/.pixi
```

> O `~/.pixi` sai junto porque o `pixi` foi instalado **para** o Mojo. Se
> outro projeto passar a usá-lo, esta linha precisa mudar — e é por isso que
> ela está escrita aqui e não só no comando agregado lá embaixo.

### Julia: por download, e a versão foi CONSULTADA

**`~/ferramentas/julia`, 288 MB comprimido, Julia 1.13.4** — para o
`p05-datalab`, onde o operador `\` resolve mínimos quadrados por QR e a
biblioteca padrão já traz `LinearAlgebra`.

Licença **MIT**, sem cadastro, sem chave.

Instalado em 2026-11-17 a partir do tarball oficial, e **não** pelo apt (o
Ubuntu empacota uma versão atrás) nem pelo `juliaup` (que instalaria um
gerenciador a mais para administrar uma única versão).

A versão não foi escolhida de memória: o script consultou
`julialang-s3.julialang.org/bin/versions.json` e pegou a mais nova marcada
como `stable`. Isso importa porque a sessão anterior deste portfólio perdeu
dois CIs por supor o nome de um arquivo de download — a convenção do Zig havia
mudado, e o `curl` sem `-f` baixou a página de erro com código de saída zero.

Remoção: `rm -rf ~/ferramentas/julia` — autocontido, nada no sistema.

### Pharo: por que por download, e não pelo apt

Nenhum Smalltalk utilizável está empacotado no Ubuntu 26.04, e isso foi
verificado antes de baixar nada:

| caminho | resultado |
|---|---|
| `gnu-smalltalk` | **não existe** — saiu do Debian por falta de manutenção |
| `squeak-vm` + `etoys` | instalam, a VM sobe (`-version` responde), mas a imagem do Etoys **ignora** o `.st` passado e nada é avaliado |

O Pharo é o descendente vivo do Smalltalk-80, é **livre (MIT)**, e tem modo de
linha de comando de primeira classe. São dois arquivos:

```bash
mkdir -p ~/pharo/vm ~/pharo/img && cd ~/pharo
curl -L -o vm.zip  https://files.pharo.org/vm/pharo-spur64-headless/Linux-x86_64/stable.zip
curl -L -o img.zip https://files.pharo.org/image/110/latest-64.zip
unzip -q vm.zip -d vm && unzip -q img.zip -d img
```

70 MB de VM e 23 MB de imagem, que descomprimidos dão 310 MB. Nada de conta,
licença ou cadastro.

## Pacotes de linguagem (pip, npm)

Ficam em caches ou em diretórios do projeto, e **não** no sistema.

| o que | onde | para quê |
|---|---|---|
| `kuzu`, `rdflib`, `owlrl`, `graphql-core` | `p10-query-lab/.venv` (44 MB) | Cypher, SPARQL e GraphQL do p10 |
| `blockly` | `~/.cache/p14-first-code` (34 MB) | Blockly do p14 |

As versões são **fixas** em `requirements.txt` e `package-lock.json`. O motivo
está nos READMEs: a suíte do p10 afirma que o SPARQL 1.1 **recusa** `{1,3}`, e
uma versão futura do rdflib que aceitasse como extensão tornaria a
documentação errada em silêncio.

### Por que o Blockly fica num cache fora de `/mnt/c`

Porque foi **medido**: com os 2302 arquivos do `node_modules` no sistema de
arquivos do Windows visto pelo WSL2, uma execução do gerador custava **1m38s**,
contra **1,7s** com as dependências no ext4. Noventa segundos de espera de
I/O para 2,8 segundos de CPU.

## Imagens Docker

| imagem | tamanho | para quê |
|---|---|---|
| `otpmesh-gateway:teste` | 8,63 MB | a imagem `scratch` do p03 — o número é o resultado |
| `golang:1.26-alpine` | 365 MB | build em dois estágios da anterior |
| `kindest/node` | 1,34 GB | o cluster local do p03 |
| `nats:2.10-alpine` | 38,7 MB | mensageria do p03 |
| `alpine:3.22` | 12,9 MB | base |
| `mcr.microsoft.com/mssql/server:2022-latest` | **2,21 GB** | **T-SQL** do p10-query-lab — SQL Server **Developer Edition**, gratuita para desenvolvimento |
| `container-registry.oracle.com/database/free:latest` | **13,8 GB** | **PL/SQL** do p10-query-lab — Oracle Database 23ai **Free**, gratuita e sem cadastro |

> As duas são as **maiores imagens do portfólio por uma ordem de grandeza**, e
> entram só para as duas linguagens que faltavam no p10. Foram registradas
> aqui **antes** de serem baixadas, pela decisão do autor de que a lista de
> remoção não fica desatualizada.
>
> **E os números acima são correções.** O registro feito antes do download
> dizia "~1,5 GB" e "~2,5 GB", de memória. O medido é **2,21 GB** e
> **13,8 GB** — o do Oracle errado por mais de cinco vezes. A estimativa foi
> substituída pelo `docker images`, e fica dito que ela era estimativa:
>
> ```
> mcr.microsoft.com/mssql/server:2022-latest          2.21GB
> container-registry.oracle.com/database/free:latest  13.8GB
> ```
>
> Uma lista de remoção com número inventado não serve para decidir nada, que é
> exatamente para o que esta lista existe.
>
> As duas edições são gratuitas: **SQL Server Developer** é licenciada para
> desenvolvimento e teste, e **Oracle Database Free** é gratuita até para
> produção, com limite de 12 GB de dados e 2 GB de RAM — nenhum deles estorva
> um banco de 33 linhas. Nenhuma das duas pediu cadastro ou chave.
>
> **Elas não cabem juntas nesta máquina**, e isso foi medido: o WSL2 expõe
> 3,8 GB, o SQL Server reserva ~2,6 GB e o Oracle pede 2 GB. Então cada
> `roda.sh` **para o outro banco antes de subir o seu**, e a suíte roda os dois
> em sequência.

## No Windows (fora do WSL)

| o que | para quê | licença |
|---|---|---|
| **AutoHotkey v2.0.31** | `Verifica-Ambiente.ahk` do p12 — lê o desktop do Windows e nomeia o que contamina uma medição | GPL-2.0 |
| **GitHub CLI** (`gh`) | publicar os repositórios e acompanhar o CI | MIT |

O AutoHotkey é *portátil*: um zip extraído, sem instalador e sem registro no
sistema. Remover é apagar a pasta.

## O que depende de CADASTRO, e por isso parou aqui

Diferente da seção seguinte: **estas são gratuitas**, não custam nada, e o
bloqueio não é dinheiro — é que instalá-las exige **criar uma conta**, e criar
conta em nome do autor é decisão dele, não minha.

| ferramenta | para quê | o que exige | medido em |
|---|---|---|---|
| **Wolfram Engine** | Wolfram Language no `p05-datalab` | **Wolfram ID** para ativar a licença (gratuita) | 2026-11-17 |
| **SAS OnDemand** | SAS no `p05-datalab` | conta SAS, e roda só na nuvem deles | 2026-11-17 |
| **PUB400** | RPG no `p13-legacy-bridge` | conta no sistema IBM i público | sessão anterior |
| **SAP BTP Trial** | ABAP no `p13-legacy-bridge` | conta SAP | sessão anterior |
| **Salesforce Dev Edition** | Apex no `p13-legacy-bridge` | conta Salesforce | sessão anterior |

O que foi verificado sobre o Wolfram, e o que não foi:

```
$ curl -I https://www.wolfram.com/engine/      ->  HTTP 200
```

A página responde e o download do Linux é público. **A ativação é que exige o
Wolfram ID** — o binário instala e se recusa a avaliar qualquer expressão sem
licença ativada. Então não há como medir "funciona" sem a conta, e eu paro
antes disso.

> Se o autor quiser essas cinco, o caminho é ele criar as contas e me dizer —
> aí as ferramentas entram, e o registro aqui passa a listar o que foi
> instalado com elas. Até então, as linguagens correspondentes ficam marcadas
> como **bloqueadas por cadastro**, e não como "a fazer", porque a diferença
> importa: nenhuma delas depende de mais trabalho meu.

### E o Mojo, que eu pensei que fosse deste grupo

O Mojo **não** exige conta, e isso foi medido antes de instalar:

```
https://conda.modular.com/max/linux-64/repodata.json   ->  HTTP 200, sem token
pacotes no canal: ['max', 'mojo', 'mojo-compiler']
```

Houve época em que a Modular pedia `modular auth` com um token do site. Hoje o
canal conda é público, e `pixi add mojo` resolve. Está registrado logo acima,
com o tamanho medido.

## O que foi recusado por ser pago

A regra do portfólio é **nenhum recurso pago**, e dois itens do plano original
batem nela:

| item | por quê | o que foi feito |
|---|---|---|
| **VBA** | exige Microsoft Office, que é pago | **feito** em LibreOffice Basic (livre), declarado como dialeto diferente — não como VBA. Ver `p12-toolbelt/basic/` |
| **Stata** | licença comercial, sem edição gratuita | declarado inviável em `VIABILIDADE.md` |

Verificado nesta máquina antes de decidir: não havia Office nem LibreOffice
instalado, e `Excel.Application`/`Word.Application` não estavam registrados.

## Desinstalando

### A condição: só quando os 14 projetos estiverem prontos E verdes no GitHub

**Nada desta seção deve ser executado antes disso**, e a razão é mecânica:
cada projeto depende do seu *toolchain* para o `verifica.sh` passar. Remover o
`ghc` quebra o p02; remover o `postgresql` quebra o p10; remover o Pharo
quebra o p14.

A condição tem três partes, e as três são **verificáveis por comando**:

```bash
# 1. os 14 projetos existem como repositório publicado
gh repo list --limit 50 --json name --jq '.[].name'

# 2. cada um com CI verde na última execução
for r in metal-vm forge-lang otp-mesh webstack datalab \
         gpu-arena engine tri-mobile silicon query-lab \
         chain toolbelt legacy-bridge first-code; do
  printf '%-16s ' "$r"
  gh run list -R "$(gh api user --jq .login)/$r" --limit 1 \
     --json conclusion --jq '.[0].conclusion'
done

# 3. a contagem de linguagens bate com a meta
lua5.4 p12-toolbelt/lua/cobertura.lua p12-toolbelt/lua/registro.lua
```

Enquanto qualquer das três falhar, a limpeza é **cedo**. E o custo de esperar
é disco, que esta máquina tem de sobra; o custo de antecipar é refazer a
instalação de um projeto inteiro para corrigir uma linha.

> Esta decisão é do autor do portfólio, registrada em 2026-11-17: *"colocar na
> lista de itens a apagar quando terminar todos os projetos e eles estarem
> certinhos no GitHub."*

### A ordem, e o que cada passo devolve

Em ordem de quanto espaço devolve. **Cada comando está escrito para ser lido
antes de rodar** — nenhum deles é reversível.

### 1. A store do Nix — 8,0 GB

```bash
# O jeito limpo: coletar o lixo e depois remover a store.
nix-collect-garbage -d
sudo apt-get purge -y nix-bin
sudo rm -rf /nix
```

A `nix-collect-garbage -d` sozinha devolve a maior parte, e é reversível no
sentido de que o flake reconstrói. O `rm -rf /nix` é definitivo.

### 2. Imagens Docker — 2,0 GB, e mais 16,0 GB com as de banco

```bash
# As duas de banco primeiro, por nome, porque sao as maiores:
docker rmi mcr.microsoft.com/mssql/server:2022-latest
docker rmi container-registry.oracle.com/database/free:latest

# E os volumes nomeados que elas criam, que o `rmi` NAO remove -- um banco
# apagado deixando o volume atras e a forma mais comum de "removi e o disco
# nao voltou".
docker volume ls --filter name=p10 -q | xargs -r docker volume rm

# Depois o resto:
docker system prune -a --volumes
sudo apt-get purge -y docker.io
```

### 3. Toolchains baixados à mão — 1,1 GB, mais 1,4 GB de Julia e Mojo

```bash
# Acrescentados em 2026-11-17/18, para o p05-datalab:
rm -rf ~/ferramentas/julia          # 288 MB, Julia 1.13.4
rm -rf ~/ferramentas/mojo           # 899 MB, Mojo 1.2.0 (ambiente pixi)
rm -rf ~/.cache/rattler ~/.pixi     # 173 MB, cache e binário do pixi
```

### Os de antes — 1,1 GB

```bash
rm -rf ~/toolchains                      # V e Odin
rm -rf ~/pharo                           # Pharo: VM e imagem
sudo rm -f /usr/local/bin/gleam /usr/local/bin/scala-cli
```

O Pharo é **autocontido**: nada dele vai para `/usr`, nada se registra no
sistema. Apagar o diretório remove a instalação inteira.

### 4. Caches — 1,1 GB

```bash
rm -rf ~/.cache/p14-first-code           # blockly
rm -rf ~/.cargo/registry ~/.hex ~/.mix
npm cache clean --force 2>/dev/null
```

### 5. Pacotes apt do portfólio — ~1 GB

```bash
sudo apt-get purge -y \
  ghc erlang elixir rebar3 openjdk-21-jdk clojure \
  dotnet-sdk-10.0 ocaml nim gnat ldc dub clang \
  rakudo groovy tcl expect lua5.4 \
  postgresql postgresql-client sqlite3 \
  fp-compiler ucblogo xvfb nodejs npm \
  squeak-vm etoys \
  libreoffice-core libreoffice-calc libreoffice-script-provider-python \
  rustc cargo rustfmt rust-clippy golang-go \
  linux-tools-common linux-tools-generic
sudo apt-get autoremove --purge -y
```

**Não** estão na lista, de propósito: `build-essential`, `cmake`,
`python3-pip`, `python3-venv` e `unzip` — são ferramentas de uso geral que
provavelmente existiam antes, ou que outra coisa na máquina usa.

### O Java tem duas versões, e só uma foi pedida

Auditado em 2026-11-17, porque a linha acima diz `openjdk-21-jdk` e o `java`
que roda **não é o 21**:

```
openjdk-21-jdk              287 MB   instalado MANUALMENTE (apt-mark showmanual)
openjdk-25-jre-headless     234 MB   entrou como DEPENDÊNCIA
default-jre-headless           —     idem

$ readlink -f $(command -v java)
/usr/lib/jvm/java-25-openjdk-amd64/bin/java
```

Ou seja: pedi o 21 (para Scala, Groovy e o Loom do p03), o apt trouxe o 25
junto, e é o **25** que o `java` resolve — então é ele que executa o
`scala-cli` e o Groovy.

Isso **não** é um furo na lista de remoção: o `apt-get autoremove --purge`
logo acima remove o 25, porque ele não é manual e nada mais vai depender dele
depois que o 21 sair. Está registrado porque a lista existe para ser
executada, e quem a ler merece saber que `purge openjdk-21-jdk` sozinho
deixaria 234 MB para trás.

E `sqlmap` e `ffuf` não são deste portfólio: foram instalados antes, em
outubro e novembro, e **não devem ser removidos por esta lista**.

### 6. Os diretórios de trabalho temporários

```bash
rm -rf ~/t14 ~/ucblogo-fonte ~/blockly-teste
```

### 7. O venv do p10

Ele mora dentro do repositório, então sai com ele:

```bash
rm -rf "p10-query-lab/.venv"
```

### 8. No Windows

O AutoHotkey portátil foi extraído em `%LOCALAPPDATA%\ahk2`:

```powershell
Remove-Item -Recurse -Force "$env:LOCALAPPDATA\ahk2"
```

O GitHub CLI é instalação normal do Windows, e sai por Configurações →
Aplicativos, se você não usar para outra coisa.

## Como saber se este arquivo está desatualizado

```bash
# Os pacotes apt instalados, com data, direto do log do sistema:
sudo zgrep -h "^Start-Date\|^Commandline: apt" /var/log/apt/history.log* \
  | paste - - | grep -i install
```

Foi assim que a tabela acima foi montada — do log, não de memória. Se um
pacote aparecer ali e não aqui, este arquivo é que está velho.
