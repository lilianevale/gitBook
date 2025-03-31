**1- Antes de tudo! O que é Controle de Versão?**

Controle de versão é um sistema que registra alterações em um arquivo ou
conjunto de arquivos ao longo do tempo para que você possa lembrar
versões específicas mais tarde. Para os exemplos neste livro você irá
utilizar o código-fonte de software com arquivos que possuam controle de
versão, embora na realidade você possa fazer isso com quase qualquer
tipo de arquivo em um computador.

Se você é um designer gráfico ou *web designer* e quer manter todas as
versões de uma imagem ou *layout* (o que você certamente deveria
querer), um sistema de controle de versão (VCS) é a coisa correta a ser
usada. Ele permite que você reverta para estado anterior determinados
arquivos ou um projeto inteiro, compare as mudanças ao longo do tempo,
veja quem modificou pela última vez algo que pode estar causando um
problema, quem introduziu um problema, quando, e muito mais. Usar um VCS
também significa que se você estragar tudo ou perder arquivos, você pode
facilmente recuperar. Além disso, você tem tudo isso com muito pouco
trabalho.

### **1.1- Sistemas Locais de Controle de Versão**

O método de controle de versão de muitas pessoas é copiar os arquivos
para outro diretório (talvez um diretório com carimbo de tempo, se eles
forem espertos). Esta abordagem é muito comum porque é muito simples,
mas também é incrivelmente propensa a erros. É fácil esquecer em qual
diretório você está e acidentalmente sobreescrever o arquivo errado ou
copiar arquivos que não quer.

Para lidar com este problema, programadores há muito tempo desenvolveram
VCSs locais que tem um banco de dados simples que mantêm todas as
alterações nos arquivos sob controle de revisão.

![](media/image2.png){width="4.317708880139983in"
height="3.053155074365704in"}

Figure 1. Controle de versão local.

Uma das ferramentas VCS mais populares foi um sistema chamado RCS, que
ainda é distribuído com muitos computadores hoje. Até mesmo o popular
sistema operacional Mac OS X inclui o comando rcs quando você instala as
Ferramentas de Desenvolvimento. RCS funciona mantendo conjuntos de
alterações (ou seja, as diferenças entre os arquivos) em um formato
especial no disco; ele pode, em seguida, re-criar como qualquer arquivo
se parecia em qualquer ponto no tempo, adicionando-se todas as
alterações.

### **1.2- Sistemas Centralizados de Controle de Versão**

A próxima questão importante que as pessoas encontram é que elas
precisam colaborar com desenvolvedores em outros sistemas. Para lidar
com este problema, Sistemas Centralizados de Controle de Versão (CVCSs)
foram desenvolvidos. Estes sistemas, tais como CVS, Subversion e
Perforce, têm um único servidor que contém todos os arquivos de controle
de versão, e um número de clientes que usam arquivos a partir desse
lugar central. Por muitos anos, este tem sido o padrão para controle de
versão.

![](media/image1.png){width="4.619792213473316in"
height="1.8801476377952755in"}

Figure 2. Controle de versão centralizado.

Esta configuração oferece muitas vantagens, especialmente sobre VCSs
locais. Por exemplo, todo mundo sabe, até certo ponto o que todo mundo
no projeto está fazendo. Os administradores têm controle refinado sobre
quem pode fazer o que; e é muito mais fácil de administrar um CVCS do
que lidar com bancos de dados locais em cada cliente.

No entanto, esta configuração também tem algumas desvantagens graves. O
mais óbvio é o ponto único de falha que o servidor centralizado
representa. Se esse servidor der problema por uma hora, durante essa
hora ninguém pode colaborar ou salvar as alterações de versão para o que
quer que eles estejam trabalhando. Se o disco rígido do banco de dados
central for corrompido, e backups apropriados não foram mantidos, você
perde absolutamente tudo - toda a história do projeto, exceto imagens
pontuais que desenvolvedores possam ter em suas máquinas locais.
Sistemas VCS locais sofrem com esse mesmo problema - sempre que você
tenha toda a história do projeto em um único lugar, há o risco de perder
tudo.

### **1.3- Sistemas Distribuídos de Controle de Versão**

É aqui que Sistemas Distribuídos de Controle de Versão (DVCS) entram em
cena. Em um DVCS (como Git, Mercurial, Bazaar ou Darcs), clientes não
somente usam o estado mais recente dos arquivos: eles duplicam
localmente o repositório completo. Assim, se qualquer servidor morrer, e
esses sistemas estiverem colaborando por meio dele, qualquer um dos
repositórios de clientes podem ser copiado de volta para o servidor para
restaurá-lo. Cada clone é de fato um backup completo de todos os dados.

![](media/image6.png){width="3.3593755468066493in"
height="4.0066961942257215in"}

Figure 3. Controle de versão distribuído.

Além disso, muitos desses sistemas trabalham muito bem com vários
repositórios remotos, tal que você possa colaborar em diferentes grupos
de pessoas de maneiras diferentes ao mesmo tempo dentro do mesmo
projeto. Isso permite que você configure vários tipos de fluxos de
trabalho que não são possíveis em sistemas centralizados, como modelos
hierárquicos.

**2- Contexto Histórico**

Como muitas coisas na vida, o Git começou com um pouco de destruição
criativa e uma ardente controvérsia.

O núcleo (kernel) do Linux é um projeto de código aberto com um escopo
bastante grande. A maior parte da vida da manutenção do núcleo o Linux
(1991-2002), as mudanças no código eram compartilhadas como correções e
arquivos. Em 2002, o projeto do núcleo do Linux começou usar uma DVCS
proprietária chamada BitKeeper.

Em 2005, a relação entre a comunidade que desenvolveu o núcleo do Linux
e a empresa que desenvolveu BitKeeper quebrou em pedaços, e a ferramenta
passou a ser paga. Isto alertou a comunidade que desenvolvia o Linux (e
especialmente Linus Torvalds, o criador do Linux) a desenvolver a sua
própria ferramenta baseada em lições aprendidas ao usar o BitKeeper.
Algumas metas do novo sistema era os seguintes:

- Velocidade

- Projeto simples

- Forte suporte para desenvolvimento não-linear (milhares de ramos
  paralelos)

- Completamente distribuído

- Capaz de lidar com projetos grandes como o núcleo o Linux com
  eficiência (velocidade e tamanho dos dados)

Desde seu nascimento em 2005, Git evoluiu e amadureceu para ser fácil de
usar e ainda reter essas qualidades iniciais. Ele é incrivelmente
rápido, é muito eficiente com projetos grandes, e ele tem um incrível
sistema de ramos para desenvolvimento não linear.

Então, em poucas palavras, o que é o Git ? Esta é uma parte que é
importante aprender, porque se você entender o que o Git é e os
fundamentos de como ele funciona, em seguida, provavelmente usar
efetivamente o Git será muito mais fácil para você. Enquanto você
estiver aprendendo sobre o Git, tente esquecer das coisas que você pode
saber sobre outros VCSs, como Subversion e Perforce; isso vai ajudá-lo a
evitar a confusão sutil ao usar a ferramenta. O Git armazena e vê
informações de forma muito diferente do que esses outros sistemas, mesmo
que a interface do usuário seja bem semelhante, e entender essas
diferenças o ajudará a não ficar confuso. (Perforce )

### **3- Principais Características**

A principal diferença entre o Git e qualquer outro VCS (Subversion e
similares) é a maneira como o Git trata seus dados. Conceitualmente, a
maioria dos outros sistemas armazenam informação como uma lista de
mudanças nos arquivos. Estes sistemas (CVS, Subversion, Perforce,
Bazaar, e assim por diante) tratam a informação como um conjunto de
arquivos e as mudanças feitas em cada arquivo ao longo do tempo.

![](media/image5.png){width="5.916666666666667in"
height="1.7964206036745407in"}

Figure 4. Armazenando dados como alterações em uma versão básica de cada
arquivo.

O Git não trata nem armazena seus dados desta forma. Em vez disso, o Git
trata seus dados mais como um conjunto de imagens de um sistema de
arquivos em miniatura. Toda vez que você fizer um *commit*, ou salvar o
estado de seu projeto no Git, ele basicamente tira uma foto de todos os
seus arquivos e armazena uma referência para esse conjunto de arquivos.
Para ser eficiente, se os arquivos não foram alterados, o Git não
armazena o arquivo novamente, apenas um link para o arquivo idêntico
anterior já armazenado. O Git trata seus dados mais como um **fluxo do
estado dos arquivos**.

![](media/image4.png){width="6.267716535433071in"
height="2.388888888888889in"}

Figure 5. Armazenando dados como um estado do conjunto de arquivos do
projeto ao longo do tempo.

Esta é uma diferença importante entre o Git e quase todos os outros
VCSs. Isto faz o Git reconsiderar quase todos os aspectos de controle de
versão que a maioria dos outros sistemas copiaram da geração anterior.
Isso faz com que o Git seja mais como um mini sistema de arquivos com
algumas ferramentas incrivelmente poderosas, ao invés de simplesmente um
VCS. Vamos explorar alguns dos benefícios que você ganha ao tratar seus
dados desta forma quando cobrirmos ramificações no Git.

### **4- Operações do Git**

A maioria das operações no Git só precisa de arquivos e recursos locais
para operar - geralmente nenhuma informação é necessária de outro
computador da rede. Se você estiver acostumado com um CVCS onde a
maioria das operações têm aquela demora causada pela latência da rede,
este aspecto do Git vai fazer você pensar que os deuses da velocidade
abençoaram o Git com poderes extraterrestres. Como você tem toda a
história do projeto ali mesmo em seu disco local, a maioria das
operações parecem quase instantâneas.

Por exemplo, para pesquisar o histórico do projeto, o Git não precisa
sair para o servidor para obter a história e exibi-lo para você - ele
simplesmente lê diretamente do seu banco de dados local. Isto significa
que você vê o histórico do projeto quase que instantaneamente. Se você
quiser ver as alterações introduzidas entre a versão atual de um arquivo
e o arquivo de um mês atrás, o Git pode procurar o arquivo de um mês
atrás e fazer um cálculo de diferença local, em vez de ter que quer
pedir a um servidor remoto para fazê-lo ou puxar uma versão mais antiga
do arquivo do servidor remoto para fazê-lo localmente.

Isto também significa que há muito pouco que você não pode fazer se você
estiver desconectado ou sem VPN. Se você estiver em um avião ou um trem
e quiser trabalhar um pouco, você pode fazer *commits* alegremente até
conseguir conexão de rede e enviar os arquivos. Se você chegar em casa e
não conseguir conectar ao VPN, você ainda poderá trabalhar. Em muitos
outros sistemas, isso é impossível ou doloroso. No Perforce, por
exemplo, você não pode fazer quase nada se você não estiver conectado ao
servidor; e no Subversion e CVS, você pode editar os arquivos, mas não
poderá enviar *commits* das alterações ao seu banco de dados (porque
você não está conectado ao seu banco de dados). Isso pode não parecer
muito, mas você poderá se surpreender com a grande diferença que isso
pode fazer.

### **5- Integridade**

Tudo no Git passa por uma soma de verificações (*checksum*) antes de ser
armazenado e é referenciado por esse *checksum*. Isto significa que é
impossível mudar o conteúdo de qualquer arquivo ou pasta sem que Git
saiba. Esta funcionalidade está incorporada no Git nos níveis mais
baixos e é parte integrante de sua filosofia. Você não perderá
informação durante a transferência e não receberá um arquivo corrompido
sem que o Git seja capaz de detectar.

O mecanismo que o Git utiliza para esta soma de verificação é chamado um
*hash* SHA-1. Esta é uma sequência de 40 caracteres composta de
caracteres hexadecimais (0-9 e-f) e é calculada com base no conteúdo de
uma estrutura de arquivo ou diretório no Git. Um *hash* SHA-1 é algo
como o seguinte:

24b9da6552252987aa493b52f8696cd6d3b00373

Você vai ver esses valores de *hash* em todo o lugar no Git porque ele
os usa com frequência. Na verdade, o Git armazena tudo em seu banco de
dados não pelo nome do arquivo, mas pelo valor de *hash* do seu
conteúdo.

### **6- Adição de Dados**

Quando você faz algo no Git, quase sempre dados são adicionados no banco
de dados do Git - e não removidos. É difícil fazer algo no sistema que
não seja reversível ou fazê-lo apagar dados de forma alguma. Como em
qualquer VCS, você pode perder alterações que ainda não tenham sido
adicionadas em um *commit*; mas depois de fazer o *commit* no Git do
estado atual das alterações, é muito difícil que haja alguma perda,
especialmente se você enviar regularmente o seu banco de dados para
outro repositório.

Isso faz com que o uso do Git seja somente alegria, porque sabemos que
podemos experimentar sem o perigo de estragar algo. Para um olhar mais
aprofundado de como o Git armazena seus dados e como você pode recuperar
dados que parecem perdidos, consulte [[Desfazendo
coisas]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/r_undoing).

### **7- Principais Estados**

Agora, preste atenção. Esta é a principal coisa a lembrar sobre Git se
você quiser que o resto do seu processo de aprendizagem ocorra sem
problemas. O Git tem três estados principais que seus arquivos podem
estar: *committed*, modificado (*modified*) e preparado (*staged*).
*Committed* significa que os dados estão armazenados de forma segura em
seu banco de dados local. Modificado significa que você alterou o
arquivo, mas ainda não fez o *commit* no seu banco de dados. Preparado
significa que você marcou a versão atual de um arquivo modificado para
fazer parte de seu próximo *commit*.

Isso nos leva a três seções principais de um projeto Git: o diretório
Git, o diretório de trabalho e área de preparo.

![](media/image3.png){width="4.973958880139983in"
height="2.7431135170603675in"}

Figure 6. Diretório de trabalho, área de preparo, e o diretório Git.

O diretório Git é onde o Git armazena os metadados e o banco de dados de
objetos de seu projeto. Esta é a parte mais importante do Git, e é o que
é copiado quando você clona um repositório de outro computador.

O diretório de trabalho é uma simples cópia de uma versão do projeto.
Esses arquivos são pegos do banco de dados compactado no diretório Git e
colocados no disco para você usar ou modificar.

A área de preparo é um arquivo, geralmente contido em seu diretório Git,
que armazena informações sobre o que vai entrar em seu próximo *commit*.
É por vezes referido como o "índice", mas também é comum referir-se a
ele como área de preparo (*staging area*).

O fluxo de trabalho básico Git é algo assim:

1.  Você modifica arquivos no seu diretório de trabalho.

2.  Você prepara os arquivos, adicionando imagens deles à sua área de
    preparo.

3.  Você faz *commit*, o que leva os arquivos como eles estão na área de
    preparo e armazena essa imagens de forma permanente para o diretório
    do Git.

Se uma versão específica de um arquivo está no diretório Git, é
considerado *commited*. Se for modificado, mas foi adicionado à área de
preparo, é considerado preparado. E se ele for alterado depois de ter
sido carregado, mas não foi preparado, ele é considerado modificado. Em
[[\[ch02-git-basics\]]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/ch02-git-basics),
você vai aprender mais sobre esses estados e como você pode tirar
proveito deles ou pular a parte de preparação inteiramente.

**8- Linha de Comandos**

Existem várias formas diferentes de usar o Git. Existem as ferramentas
originais de linha de comando, e existem várias interfaces gráficas de
usuário (GUI - Graphical User Interface) com opções variadas. Para este
livro, nós usaremos o Git na linha de comando. A linha de comando é o
único lugar onde você pode rodar **todos** os comandos do Git - a
maioria das GUI implementa somente um subconjunto das funcionalidades do
Git. Se você sabe como usar o Git na linha de comando, você
provavelmente descobrirá como rodar versões GUI, enquanto o oposto não é
necessariamente verdade. Além disso, enquanto a sua escolha da interface
gráfica é uma questão de gosto pessoal, *todos* os usuário terão as
ferramentas de linha de comando instaladas e disponíveis.

Então, nós esperamos que você saiba como abrir um Terminal no Mac, ou
Linha de Comando ou Powershell no Windows. Se você não sabe do que
estamos falando, talvez você precise parar e pesquisar isso antes de
continuar, para poder seguir os exemplos descritos neste livro.

**9- Instalação**

**9.1- LINUX**

Debian/Ubuntu

Para a versão estável mais recente para seu lançamento do Debian/Ubuntu

\# apt-get install git

Para o Ubuntu, este PPA fornece a versão estável mais recente do Git
upstream

\# add-apt-repository ppa:git-core/ppa

\# apt update; apt install git

Fedora

\# yum install git (até Fedora 21)

\# dnf install git (Fedora 22 e posterior)

Gentoo

\# emerge \--ask \--verbose dev-vcs/git

Arch Linux

\# pacman -S git

openSUSE

\# zypper install git

Mageia

\# urpmi git

Nix/NixOS

\# nix-env -i git

FreeBSD

\# pkg install git

Solaris 9/10/11 (OpenCSW)

\# pkgutil -i git

Solaris 11 Express, OpenIndiana

\# pkg install developer/versioning/git

OpenBSD

\# pkg_add git

Alpine

\$ apk add git

Red Hat Enterprise Linux, Oracle Linux, CentOS, Scientific Linux, et al.

RHEL e derivados normalmente fornecem versões mais antigas do git. Você
pode baixar um tarball e construir a partir do código-fonte, ou usar um
repositório de terceiros, como o IUS Community Project, para obter uma
versão mais recente do git.

### **9.2- Windows**

Há também algumas maneiras de instalar o Git no Windows. A compilação
mais oficial está disponível para download no site do Git. Basta ir ao
[[https://git-scm.com/download/win]{.underline}](https://git-scm.com/download/win)
e o download começará automaticamente. Note que este é um projeto
chamado Git para Windows (também chamado msysGit), que é algo separado
do Git; para mais informações sobre isso, vá para
[[http://msysgit.github.io/]{.underline}](http://msysgit.github.io/).

Para fazer uma instalação automatizada, você pode usar o [[pacote Git do
Chocolatey]{.underline}](https://chocolatey.org/packages/git). Note que
o pacote Chocolatey é mantido pela comunidade.

Outra forma fácil de obter Git instalada é através da instalação de
GitHub para Windows. O instalador inclui uma versão de linha de comando
do Git, bem como a GUI. Ele também funciona bem com o PowerShell, e
configura o cache de credenciais sólidas e as devidas configurações
CRLF. Vamos saber mais sobre isso um pouco mais tarde, por enquanto
basta dizer que estas são coisas que você deveria ter. Você pode
baixá-lo da página GitHub para Windows, em
[[http://windows.github.com]{.underline}](http://windows.github.com).

### **9.3- Instalando da Fonte**

Algumas pessoas podem achar interessante instalar Git a partir da fonte,
para ter a versão mais recente. Os instaladores binários tendem a ficar
um pouco atrás, embora após o Git ter amadurecido nos últimos anos, isso
faz cada vez menos diferença.

Se você deseja instalar o Git a partir da fonte, você precisa ter as
seguintes bibliotecas das quais o Git: curl, zlib, openssl, expat, e
libiconv. Por exemplo, se você estiver em um sistema que tem yum (como o
Fedora) ou apt-get (tal como um sistema baseado em Debian), você pode
usar um destes comandos para instalar as dependências mínimas para
compilar e instalar os binários do Git:

source,console\]

\$ sudo yum install dh-autoreconf curl-devel expat-devel gettext-devel
\\

openssl-devel perl-devel zlib-devel

\$ sudo apt-get install dh-autoreconf libcurl4-gnutls-dev libexpat1-dev
\\

gettext libz-dev libssl-dev

Para incluir a documentação em vários formatos (doc, html, info), essas
dependências adicionais são necessárias:

\$ sudo yum install asciidoc xmlto docbook2X getopt

\$ sudo apt-get install asciidoc xmlto docbook2x getopt

Além disso, se estiver usando o Fedora/RHEL/derivados-do-RHEL, vai
precisar executar

\$ sudo ln -s /usr/bin/db2x_docbook2texi /usr/bin/docbook2x-texi

devido a diferenças nos nomes dos binários.

Quando você tiver todas as dependências necessárias, você poderá baixar
o tarball com a última versão de vários lugares. Você pode obtê-lo
através da página Kernel.org, em
[[https://www.kernel.org/pub/software/scm/git]{.underline}](https://www.kernel.org/pub/software/scm/git),
ou no espelho no site do GitHub, em
[[https://github.com/git/git/releases]{.underline}](https://github.com/git/git/releases).
Em geral, é um pouco mais claro qual é a versão mais recente na página
do GitHub, mas a página kernel.org também tem assinaturas se você quiser
verificar o seu download.

Então, compile e instale:

\$ tar -zxf git-2.0.0.tar.gz

\$ cd git-2.0.0

\$ make configure

\$ ./configure \--prefix=/usr

\$ make all doc info

\$ sudo make install install-doc install-html install-info

Depois de ter feito isso, você poderá atualizar o Git através dele
mesmo:

\$ git clone git://git.kernel.org/pub/scm/git/git.git

## **10- Configuração Inicial do Git**

Agora que você tem o Git em seu sistema, você deve fazer algumas coisas
para personalizar o ambiente Git. Você fará isso apenas uma vez por
computador e o efeito se manterá após atualizações. Você também pode
mudá-las em qualquer momento rodando esses comandos novamente.

O Git vem com uma ferramenta chamada git config que permite ver e
atribuir variáveis de configuração que controlam todos os aspectos de
como o Git aparece e opera. Estas variáveis podem ser armazenadas em
três lugares diferentes:

1.  /etc/gitconfig: válido para todos os usuários no sistema e todos os
    seus repositórios. Se você passar a opção \--system para git config,
    ele lê e escreve neste arquivo.

2.  \~/.gitconfig ou \~/.config/git/config: Somente para o seu usuário.
    Você pode fazer o Git ler e escrever neste arquivo passando a opção
    \--global.

3.  config no diretório Git (ou seja, .git/config) de qualquer
    repositório que você esteja usando: específico para este
    repositório.

Cada nível sobrescreve os valores no nível anterior, ou seja, valores em
.git/config prevalecem sobre /etc/gitconfig.

No Windows, Git procura pelo arquivo .gitconfig no diretório \$HOME
(C:\\Users\\\$USER para a maioria). Ele também procura por
/etc/gitconfig, mesmo sendo relativo à raiz do sistema, que é onde quer
que você tenha instalado Git no seu sistema.

### **10.2- Sua Identidade**

A primeira coisa que você deve fazer ao instalar Git é configurar seu
nome de usuário e endereço de e-mail. Isto é importante porque cada
*commit* usa esta informação, e ela é carimbada de forma imutável nos
*commits* que você começa a criar:

\$ git config \--global user.name \"Fulano de Tal\"

\$ git config \--global user.email fulanodetal@exemplo.br

Reiterando, você precisará fazer isso somente uma vez se tiver usado a
opção \--global, porque então o Git usará esta informação para qualquer
coisa que você fizer naquele sistema. Se você quiser substituir essa
informação com nome diferente para um projeto específico, você pode
rodar o comando sem a opção \--global dentro daquele projeto.

Muitas ferramentas GUI o ajudarão com isso quando forem usadas pela
primeira vez.

### **10.3- O Editor**

Agora que a sua identidade está configurada, você pode escolher o editor
de texto padrão que será chamado quando Git precisar que você entre uma
mensagem. Se não for configurado, o Git usará o editor padrão, que
normalmente é o Vim. Se você quiser usar um editor de texto diferente,
como o Emacs, você pode fazer o seguinte:

\$ git config \--global core.editor emacs

### **10.4- Testando as Configurações**

Se você quiser testar as suas configurações, você pode usar o comando
git config \--list para listar todas as configurações que o Git
conseguir encontrar naquele momento:

\$ git config \--list

user.name=Fulano de Tal

user.email=fulanodetal@exemplo.br

color.status=auto

color.branch=auto

color.interactive=auto

color.diff=auto

\...

Pode ser que algumas palavras chave apareçam mais de uma vez, porque Git
lê as mesmas chaves de arquivos diferentes (/etc/gitconfig e
\~/.gitconfig, por exemplo). Neste caso, Git usa o último valor para
cada chave única que ele vê.

Você pode também testar o que Git tem em uma chave específica digitando
git config \<key\>:

\$ git config user.name

Fulano de Tal

11- Pedindo Ajuda

Se você precisar de ajuda para usar o Git, há três formas de acessar a
página do manual de ajuda (*manpage*) para qualquer um dos comandos Git:

\$ git help \<verb\>

\$ git \<verb\> \--help

\$ man git-\<verb\>

Por exemplo, você pode ver a *manpage* do commando config rodando

\$ git help config

Estes comandos podem ser acessados de qualquer lugar, mesmo
desconectado. Se as *manpages* e este livro não forem suficientes e você
precisar de ajuda personalizada, você pode tentar os canais #git ou
#github no servidor IRC Freenode (irc.freenode.net). Estes canais estão
sempre cheios com centenas de pessoas que são bem versadas com o Git e
dispostas a ajudar.
