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
| imagens Docker | 2,0 GB | do p03-otp-mesh (Go em `scratch`, kind, NATS) |
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

## No Windows (fora do WSL)

| o que | para quê | licença |
|---|---|---|
| **AutoHotkey v2.0.31** | `Verifica-Ambiente.ahk` do p12 — lê o desktop do Windows e nomeia o que contamina uma medição | GPL-2.0 |
| **GitHub CLI** (`gh`) | publicar os repositórios e acompanhar o CI | MIT |

O AutoHotkey é *portátil*: um zip extraído, sem instalador e sem registro no
sistema. Remover é apagar a pasta.

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

### 2. Imagens Docker — 2,0 GB

```bash
docker system prune -a --volumes
sudo apt-get purge -y docker.io
```

### 3. Toolchains baixados à mão — 1,1 GB

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
