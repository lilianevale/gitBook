**2- Clonando um repositório**

Você pode obter um projeto Git utilizando duas formas principais. 1.
Você pode pegar um diretório local que atualmente não está sob controle
de versão e transformá-lo em um repositório Git, ou 2. Você pode fazer
um *clone* de um repositório Git existente em outro lugar.

### **2.1- Inicializando um Repositório em um Diretório Existente**

Para você começar a monitorar um projeto existente com Git, você deve ir
para o diretório desse projeto. Se você nunca fez isso, use o comando a
seguir, que terá uma pequena diferença dependendo do sistema em que está
executando:

para Linux:

\$ cd /home/user/your_repository

para Mac:

\$ cd /Users/user/your_repository

para Windows:

\$ cd /c/user/your_repository

depois digite:

\$ git init

Isso cria um novo subdiretório chamado .git que contém todos os arquivos
necessários de seu repositório -- um esqueleto de repositório Git. Neste
ponto, nada em seu projeto é monitorado ainda. (Veja
[[\[ch10-git-internals\]]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/ch10-git-internals)
para mais informações sobre quais arquivos estão contidos no diretório
.git que foi criado.)

Se você quer começar a controlar o versionamento dos arquivos existentes
(ao contrário de um diretório vazio), você provavelmente deve começar a
monitorar esses arquivos e fazer um *commit* inicial. Você pode fazer
isso com alguns comandos git add que especificam os arquivos que você
quer monitorar, seguido de um git commit:

\$ git add \*.c

\$ git add LICENSE

\$ git commit -m \'initial project version\'

Nós já veremos o que esses comandos fazem. Mas neste ponto você já tem
um repositório Git com arquivos monitorados e um *commit* inicial.

![](media/image3.png){width="6.267716535433071in"
height="2.763888888888889in"}

Figure 8. O ciclo de vida dos status de seus arquivos.

### **2.2- Verificando os Status de Seus Arquivos**

A principal ferramenta que você vai usar para determinar quais arquivos
estão em qual estado é o comando git status. Se você executar esse
comando imediatamente após clonar um repositório, você vai ver algo
assim:

\$ git status

On branch master

Your branch is up-to-date with \'origin/master\'.

nothing to commit, working directory clean

Isso significa que você tem um diretório de trabalho limpo - em outras
palavras, nenhum de seus arquivos rastreados foi modificado. O Git
também não está vendo nenhum arquivo não rastreado, senão eles estariam
listados aqui. Finalmente, o comando lhe diz qual o branch que você está
e diz que ele não divergiu do mesmo branch no servidor. Por enquanto,
esse branch é sempre "master", que é o padrão; você não precisa se
preocupar com isso agora.
[[\[ch03-git-branching\]]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/ch03-git-branching)
vai abordar branches e referências em detalhe.

Digamos que você adiciona um novo arquivo no seu projeto, um simples
arquivo README. Se o arquivo não existia antes, e você executar git
status, você verá seu arquivo não rastreado da seguinte forma:

\$ echo \'My Project\' \> README

\$ git status

On branch master

Your branch is up-to-date with \'origin/master\'.

Untracked files:

(use \"git add \<file\>\...\" to include in what will be committed)

README

nothing added to commit but untracked files present (use \"git add\" to
track)

Você pode ver que o seu novo arquivo README é um arquivo não rastreado,
porque está abaixo do subtítulo "Untracked files" na saída do seu
status. \"Não rastreado\" basicamente significa que o Git vê um arquivo
que você não tinha no snapshot (commit) anterior; o Git não vai passar a
incluir o arquivo nos seus commits a não ser que você o mande fazer isso
explicitamente. O comportamento do Git é dessa forma para que você não
inclua acidentalmente arquivos binários gerados automaticamente ou
outros arquivos que você não deseja incluir. Você *quer* incluir o
arquivo README, então vamos comeaçar a rastreá-lo.

### **2.3- Rastreando Arquivos Novos**

Para começar a rastrear um novo arquivo, você deve usar o comando git
add. Para começar a rastrear o arquivo README, você deve executar o
seguinte:

\$ git add README

Executando o comando *status* novamente, você pode ver que seu README
agora está sendo rastreado e preparado (*staged*) para o *commit*:

\$ git status

On branch master

Your branch is up-to-date with \'origin/master\'.

Changes to be committed:

(use \"git reset HEAD \<file\>\...\" to unstage)

new file: README

É possível saber que o arquivo está preparado porque ele aparece sob o
título "Changes to be committed". Se você fizer um *commit* neste
momento, a versão do arquivo que existia no instante em que você
executou git add, é a que será armazenada no histórico de *snapshots*.
Você deve se lembrar que, quando executou git init anteriormente, em
seguida, você também executou git add (arquivos) - isso foi para começar
a rastrear os arquivos em seu diretório. O comando git add recebe o
caminho de um arquivo ou de um diretório. Se for um diretório, o comando
adiciona todos os arquivos contidos nesse diretório recursivamente.

### **2.4- Preparando Arquivos Modificados**

Vamos modificar um arquivo que já estava sendo rastreado. Se você
modificar o arquivo CONTRIBUTING.md, que já era rastreado, e então
executar git status novamente, você deve ver algo como:

\$ git status

On branch master

Your branch is up-to-date with \'origin/master\'.

Changes to be committed:

(use \"git reset HEAD \<file\>\...\" to unstage)

new file: README

Changes not staged for commit:

(use \"git add \<file\>\...\" to update what will be committed)

(use \"git checkout \-- \<file\>\...\" to discard changes in working
directory)

modified: CONTRIBUTING.md

O arquivo CONTRIBUTING.md aparece sob a seção "Changes not staged for
commit" --- que indica que um arquivo rastreado foi modificado no
diretório de trabalho mas ainda não foi mandado para o *stage*
(preparado). Para mandá-lo para o *stage*, você precisa executar o
comando git add. O git add é um comando de múltiplos propósitos: serve
para começar a rastrear arquivos e também para outras coisas, como
marcar arquivos que estão em conflito de mesclagem como resolvidos. Pode
ser útil pensar nesse comando mais como "adicione este conteúdo ao
próximo *commit*". Vamos executar git add agora, para mandar o arquivo
CONTRIBUTING.md para o *stage*, e então executar git status novamente:

\$ git add CONTRIBUTING.md

\$ git status

On branch master

Your branch is up-to-date with \'origin/master\'.

Changes to be committed:

(use \"git reset HEAD \<file\>\...\" to unstage)

new file: README

modified: CONTRIBUTING.md

Ambos os arquivos estão preparados (no *stage*) e entrarão no seu
próximo *commit*. Neste momento, suponha que você se lembre de uma
pequena mudança que quer fazer no arquivo CONTRIBUTING.md antes de fazer
o *commit*. Você abre o arquivo, faz a mudança e está pronto para fazer
o *commit*. No entanto, vamos executar git status mais uma vez:

\$ vim CONTRIBUTING.md

\$ git status

On branch master

Your branch is up-to-date with \'origin/master\'.

Changes to be committed:

(use \"git reset HEAD \<file\>\...\" to unstage)

new file: README

modified: CONTRIBUTING.md

Changes not staged for commit:

(use \"git add \<file\>\...\" to update what will be committed)

(use \"git checkout \-- \<file\>\...\" to discard changes in working
directory)

modified: CONTRIBUTING.md

Que negócio é esse? Agora o CONTRIBUTING.md está listado como preparado
(*staged*) e também como não-preparado (*unstaged*). Como isso é
possível? Acontece que o Git põe um arquivo no *stage* exatamente como
ele está no momento em que você executa o comando git add. Se você
executar git commit agora, a versão do CONTRIBUTING.md que vai para o
repositório é aquela de quando você executou git add, não a versão que
está no seu diretório de trabalho. Se você modificar um arquivo depois
de executar git add, você tem que executar git add de novo para por sua
versão mais recente no *stage*:

\$ git add CONTRIBUTING.md

\$ git status

On branch master

Your branch is up-to-date with \'origin/master\'.

Changes to be committed:

(use \"git reset HEAD \<file\>\...\" to unstage)

new file: README

modified: CONTRIBUTING.md

### **2.5 Status** 

Ao mesmo tempo que a saída do git status é bem completa, ela também é
bastante verbosa. O Git também tem uma *flag* para status curto, que
permite que você veja suas alterações de forma mais compacta. Se você
executar git status -s ou git status \--short a saída do comando vai ser
bem mais simples:

\$ git status -s

M README

MM Rakefile

A lib/git.rb

M lib/simplegit.rb

?? LICENSE.txt

Arquivos novos que não são rastreados têm um ?? do lado, novos arquivos
que foram adicionados à área de *stage* têm um A, arquivos modificados
têm um M e assim por diante. Há duas colunas de status na saída: a
coluna da esquerda indica o status da área de *stage* e a coluna da
direita indica o status do diretório de trabalho. No exemplo anterior, o
arquivo README foi modificado no diretório de trabalho mas ainda não foi
para o *stage*, enquanto o arquivo lib/simplegit.rb foi modificado e foi
para o *stage*. O arquivo Rakefile foi modificado, foi para o *stage* e
foi modificado de novo, de maneira que há alterações para ele tanto no
estado preparado quanto no estado não-preparado.

### **2.6- Ignorando Arquivos**

Frequentemente você terá uma classe de arquivos que não quer que sejam
adicionados automaticamente pelo Git e nem mesmo que ele os mostre como
não-rastreados. Geralmente, esses arquivos são gerados automaticamente,
tais como arquivos de *log* ou arquivos produzidos pelo seu sistema de
compilação (*build*). Nesses casos, você pode criar um arquivo chamado
.gitignore, contendo uma lista de padrões de nomes de arquivo que devem
ser ignorados. Aqui está um exemplo de arquivo .gitignore:

\$ cat .gitignore

\*.\[oa\]

\*\~

A primeira linha diz ao Git para ignorar todos os arquivos que terminem
com ".o" ou ".a" -- arquivos objeto ou de arquivamento, que podem ser
produtos do processo de compilação. A segunda linha diz ao Git para
ignorar todos os arquivos cujo nome termine com um til (\~), que é usado
por muitos editores de texto, como o Emacs, para marcar arquivos
temporários. Você também pode incluir diretórios log, tmp ou pid;
documentação gerada automaticamente; e assim por diante. Configurar um
arquivo .gitignore, antes de você começar um repositório, geralmente é
uma boa ideia para que você não inclua acidentalmente em seu repositório
Git arquivos que você não quer.

As regras para os padrões que podem ser usados no arquivo .gitignore são
as seguintes:

- Linhas em branco ou começando com \# são ignoradas.

- Os padrões que normalmente são usados para nomes de arquivos
  funcionam.

- Você pode iniciar padrões com uma barra (/) para evitar recursividade.

- Você pode terminar padrões com uma barra (/) para especificar um
  diretório.

- Você pode negar um padrão ao fazê-lo iniciar com um ponto de
  exclamação (!).

Padrões de nome de arquivo são como expressões regulares simplificadas
usadas em ambiente *shell*. Um asterisco (\*) casa com zero ou mais
caracteres; \[abc\] casa com qualquer caracter dentro dos colchetes
(neste caso, a, b ou c); um ponto de interrogação (?) casa com um único
caracter qualquer; e caracteres entre colchetes separados por hífen
(\[0-9\]) casam com qualquer caracter entre eles (neste caso, de 0 a 9).
Você também pode usar dois asteriscos para criar uma expressão que case
com diretórios aninhados; a/\*\*/z casaria com a/z, a/b/z, a/b/c/z, e
assim por diante.

Aqui está outro exemplo de arquivo .gitignore:

\# ignorar arquivos com extensão .a

\*.a

\# mas rastrear o arquivo lib.a, mesmo que você esteja ignorando os
arquivos .a acima

!lib.a

\# ignorar o arquivo TODO apenas no diretório atual, mas não em
subdir/TODO

/TODO

\# ignorar todos os arquivos no diretório build/

build/

\# ignorar doc/notes.txt, mas não doc/server/arch.txt

doc/\*.txt

\# ignorar todos os arquivos .pdf no diretório doc/

doc/\*\*/\*.pdf

### **2.7- Visualizando as Alterações Dentro e Fora do Stage**

Se o comando git status for vago demais para você --- você quer saber
exatamente o que você alterou, não apenas quais arquivos foram
alterados --- você pode usar o comando git diff. Nós explicaremos o git
diff em detalhes mais tarde, mas provavelmente você vai usá-lo com maior
frequência para responder a essas duas perguntas: O que você alterou mas
ainda não mandou para o *stage* (estado preparado)? E o que está no
*stage*, pronto para o *commit*? Apesar de o git status responder a
essas perguntas de forma genérica, listando os nomes dos arquivos, o git
diff exibe exatamente as linhas que foram adicionadas e removidas --- o
*patch*, como costumava se chamar.

Digamos que você altere o arquivo README e o mande para o *stage* e
então altere o arquivo CONTRIBUTING.md sem mandá-lo para o *stage*. Se
você executar o comando git status, você verá mais uma vez alguma coisa
como o seguinte:

\$ git status

On branch master

Your branch is up-to-date with \'origin/master\'.

Changes to be committed:

(use \"git reset HEAD \<file\>\...\" to unstage)

modified: README

Changes not staged for commit:

(use \"git add \<file\>\...\" to update what will be committed)

(use \"git checkout \-- \<file\>\...\" to discard changes in working
directory)

modified: CONTRIBUTING.md

Para ver o que você alterou mas ainda não mandou para o *stage*, digite
o comando git diff sem nenhum argumento:

\$ git diff

diff \--git a/CONTRIBUTING.md b/CONTRIBUTING.md

index 8ebb991..643e24f 100644

\-\-- a/CONTRIBUTING.md

+++ b/CONTRIBUTING.md

@@ -65,7 +65,8 @@ branch directly, things can get messy.

Please include a nice description of your changes when you submit your
PR;

if we have to read the whole diff to figure out why you\'re contributing

in the first place, you\'re less likely to get feedback and have your
change

-merged in.

+merged in. Also, split your changes into comprehensive chunks if your
patch is

+longer than a dozen lines.

If you are starting to work on a particular area, feel free to submit a
PR

that highlights your work in progress (and note in the PR title that
it\'s

Esse comando compara o que está no seu diretório de trabalho com o que
está no *stage*. O resultado permite que você saiba quais alterações
você fez que ainda não foram mandadas para o *stage*.

Se você quiser ver as alterações que você mandou para o *stage* e que
entrarão no seu próximo *commit*, você pode usar git diff \--staged.
Este comando compara as alterações que estão no seu *stage* com o seu
último *commit*:

\$ git diff \--staged

diff \--git a/README b/README

new file mode 100644

index 0000000..03902a1

\-\-- /dev/null

+++ b/README

@@ -0,0 +1 @@

+My Project

É importante notar que o git diff sozinho não mostra todas as alterações
feitas desde o seu último *commit* --- apenas as alterações que ainda
não estão no *stage* (não-preparado). Isso pode ser confuso porque, se
você já tiver mandado todas as suas alterações para o *stage*, a saída
do git diff vai ser vazia.

Um outro exemplo: se você mandar o arquivo CONTRIBUTING.md para o
*stage* e então alterá-lo, você pode usar o git diff para ver as
alterações no arquivo que estão no *stage* e também as que não estão. Se
o nosso ambiente se parecer com isso:

\$ git add CONTRIBUTING.md

\$ echo \'# test line\' \>\> CONTRIBUTING.md

\$ git status

On branch master

Your branch is up-to-date with \'origin/master\'.

Changes to be committed:

(use \"git reset HEAD \<file\>\...\" to unstage)

modified: CONTRIBUTING.md

Changes not staged for commit:

(use \"git add \<file\>\...\" to update what will be committed)

(use \"git checkout \-- \<file\>\...\" to discard changes in working
directory)

modified: CONTRIBUTING.md

Agora você poderá usar o git diff para ver o que ainda não foi mandado
para o *stage*:

\$ git diff

diff \--git a/CONTRIBUTING.md b/CONTRIBUTING.md

index 643e24f..87f08c8 100644

\-\-- a/CONTRIBUTING.md

+++ b/CONTRIBUTING.md

@@ -119,3 +119,4 @@ at the

\## Starter Projects

See our \[projects
list\](https://github.com/libgit2/libgit2/blob/development/PROJECTS.md).

+# test line

e git diff \--cached para ver o que você já mandou para o *stage* até
agora (\--staged e \--cached são sinônimos):

\$ git diff \--cached

diff \--git a/CONTRIBUTING.md b/CONTRIBUTING.md

index 8ebb991..643e24f 100644

\-\-- a/CONTRIBUTING.md

+++ b/CONTRIBUTING.md

@@ -65,7 +65,8 @@ branch directly, things can get messy.

Please include a nice description of your changes when you submit your
PR;

if we have to read the whole diff to figure out why you\'re contributing

in the first place, you\'re less likely to get feedback and have your
change

-merged in.

+merged in. Also, split your changes into comprehensive chunks if your
patch is

+longer than a dozen lines.

If you are starting to work on a particular area, feel free to submit a
PR

that highlights your work in progress (and note in the PR title that
it\'s

### **2.8- Fazendo Commit das Suas Alterações**

Agora que sua área de *stage* está preparada do jeito que você quer,
você pode fazer *commit* das suas alterações. Lembre-se que qualquer
coisa que ainda não foi enviada para o *stage* --- qualquer arquivo que
você tenha criado ou alterado e que ainda não tenha sido adicionado com
git add --- não entrará nesse *commit*. Esses arquivos permanecerão no
seu disco como arquivos alterados. Nesse caso, digamos que, da última
vez que você executou git status, você viu que tudo estava no *stage*,
então você está pronto para fazer *commit* de suas alterações. O jeito
mais simples de fazer *commit* é digitar git commit:

\$ git commit

Fazendo isso, será aberto o editor de sua escolha.

O editor mostra o seguinte texto (este é um exemplo da tela do Vim):

\# Please enter the commit message for your changes. Lines starting

\# with \'#\' will be ignored, and an empty message aborts the commit.

\# On branch master

\# Your branch is up-to-date with \'origin/master\'.

\#

\# Changes to be committed:

\# new file: README

\# modified: CONTRIBUTING.md

\#

\~

\~

\~

\".git/COMMIT_EDITMSG\" 9L, 283C

Você pode ver que a mensagem de *commit* padrão contém a saída mais
recente do comando git status, comentada, e uma linha em branco no topo.
Você pode remover esses comentários e digitar sua mensagem de *commit*,
ou você pode deixá-los lá para ajudá-lo a lembrar o que faz parte do
*commit*.

Para um lembrete ainda mais explícito do que você alterou, você pode
passar a opção -v para o git commit. Isso inclui as diferenças (*diff*)
da sua alteração no editor, para que você possa ver exatamente quais
alterações estão entrando no *commit*.

Quando você sair do editor, o Git criará seu *commit* com essa mensagem
(com os comentários e diferenças removidos).

Alternativamente, você pode digitar sua mensagem de *commit* diretamente
na linha de comando, depois da opção -m do comando commit, assim:

\$ git commit -m \"Story 182: Fix benchmarks for speed\"

\[master 463dc4f\] Story 182: Fix benchmarks for speed

2 files changed, 2 insertions(+)

create mode 100644 README

Você acaba de criar seu primeiro *commit*! Veja que a saída do comando
fornece algumas informações: em qual *branch* foi feito o *commit*
(master), seu *checksum* SHA-1 (463dc4f), quantos arquivos foram
alterados e estatísticas sobre o número de linhas adicionadas e
removidas.

Lembre-se de que o *commit* grava o *snapshot* que você deixou na área
de *stage*. Qualquer alteração que você não tiver mandado para o *stage*
permanecerá como estava, em seu lugar; você pode executar outro *commit*
para adicioná-la ao seu histórico. Toda vez que você executa um
*commit*, você está gravando um *snapshot* do seu projeto que você pode
usar posteriormente para fazer comparações, ou mesmo restaurá-lo.

### **2.9- Pulando a Área de Stage**

Mesmo sendo incrivelmente útil para preparar *commits* exatamente como
você quer, a área de *stage* algumas vezes é um pouco mais complexa do
que o necessário. Se você quiser pular a área de *stage*, o Git fornece
um atalho simples. A opção -a, do comando git commit, faz o Git mandar
todos arquivos rastreados para o *stage* automaticamente, antes de fazer
o *commit*, permitindo que você pule a parte do git add:

\$ git status

On branch master

Your branch is up-to-date with \'origin/master\'.

Changes not staged for commit:

(use \"git add \<file\>\...\" to update what will be committed)

(use \"git checkout \-- \<file\>\...\" to discard changes in working
directory)

modified: CONTRIBUTING.md

no changes added to commit (use \"git add\" and/or \"git commit -a\")

\$ git commit -a -m \'added new benchmarks\'

\[master 83e38c7\] added new benchmarks

1 file changed, 5 insertions(+), 0 deletions(-)

Perceba que, nesse caso, você não tem que executar git add antes, para
adicionar o arquivo CONTRIBUTING.md ao *commit*. Isso ocorre porque a
opção -a inclui todos os arquivos alterados. Isso é conveniente, mas
cuidado; algumas vezes esta opção fará você incluir alterações
indesejadas.

### **2.10- Removendo Arquivos**

Para remover um arquivo do Git, você tem que removê-lo dos seus arquivos
rastreados (mais precisamente, removê-lo da sua área de *stage*) e então
fazer um *commit*. O comando git rm faz isso, e também remove o arquivo
do seu diretório de trabalho para que você não o veja como um arquivo
não-rastreado nas suas próximas interações.

Se você simplesmente remover o arquivo do seu diretório, ele aparecerá
sob a área "Changes not staged for commit" (isto é, fora do *stage*) da
saída do seu git status:

\$ rm PROJECTS.md

\$ git status

On branch master

Your branch is up-to-date with \'origin/master\'.

Changes not staged for commit:

(use \"git add/rm \<file\>\...\" to update what will be committed)

(use \"git checkout \-- \<file\>\...\" to discard changes in working
directory)

deleted: PROJECTS.md

no changes added to commit (use \"git add\" and/or \"git commit -a\")

Mas, se você executar git rm, o arquivo será preparado para remoção
(retirado do *stage*):

\$ git rm PROJECTS.md

rm \'PROJECTS.md\'

\$ git status

On branch master

Your branch is up-to-date with \'origin/master\'.

Changes to be committed:

(use \"git reset HEAD \<file\>\...\" to unstage)

deleted: PROJECTS.md

Da próxima vez que você fizer um *commit*, o arquivo será eliminado e
não será mais rastreado. Se o arquivo tiver sido alterado ou se já tiver
adicionado à área de *stage*, você terá que forçar a remoção com a opção
-f. Essa é uma medida de segurança para prevenir a exclusão acidental de
dados que ainda não tenham sido gravados em um *snapshot* e que não
poderão ser recuperados do histórico.

Outra coisa útil que você pode querer fazer é manter o arquivo no seu
diretório de trabalho, mas removê-lo da sua área de *stage*. Em outras
palavras, você pode querer manter o arquivo no seu disco rígido, mas não
deixá-lo mais sob controle do Git. Isso é particularmente útil se você
esquecer de adicionar alguma coisa ao seu arquivo .gitignore e,
acidentalmente, mandá-la para o *stage*, como um grande arquivo de *log*
ou um monte de arquivos compilados .a. Para fazer isso, use a opção
\--cached:

\$ git rm \--cached README

Você pode passar arquivos, diretórios e padrões de nomes para o comando
git rm. Isso quer dizer que você pode fazer coisas como:

\$ git rm log/\\\*.log

Note a barra invertida (\\) na frente do \*. Isso é necessário porque o
Git faz sua própria expansão de nomes de arquivos em adição a que é
feita pela sua *shell*. Esse comando remove todos os arquivos que tenham
a extensão .log do diretório log/. Ou, você pode fazer algo como o
seguinte:

\$ git rm \\\*\~

Esse comando remove todos os arquivos cujos nomes terminem com um \~.

### **2.11- Movendo Arquivos**

Diferentemente de outros sistemas de controle de versão, o Git não
rastreia explicitamente a movimentação de arquivos. Se você renomear um
arquivo no Git, ele não armazena metadados indicando que determinado
arquivo foi renomeado. Porém, o Git é bastante esperto para perceber
isso depois do fato ocorrido --- nós trataremos de movimentação de
arquivos daqui a pouco.

Assim, é um pouco confuso o fato de o Git ter um comando mv. Se você
quiser renomear um arquivo no Git, você pode executar alguma coisa como:

\$ git mv arq_origem arq_destino

e vai funcionar bem. Na verdade, se você executar alguma coisa assim e
verificar o *status*, você vai ver que o Git considera que arquivo foi
renomeado:

\$ git mv README.md README

\$ git status

On branch master

Your branch is up-to-date with \'origin/master\'.

Changes to be committed:

(use \"git reset HEAD \<file\>\...\" to unstage)

renamed: README.md -\> README

Contudo, isso é equivalente a executar algo como:

\$ mv README.md README

\$ git rm README.md

\$ git add README

O Git percebe que, implicitamente, se trata de um arquivo renomeado, de
maneira que não importa se você renomear um arquivo desse jeito ou com o
comando mv. A única diferença real é que git mv é um comando em vez de
três --- é uma função de conveniência. Mais importante, você pode usar
qualquer ferramenta que quiser para renomear um arquivo e cuidar do
add/rm depois, antes de fazer o *commit*.

## **3- Histórico de Commits**

Depois de você ter criado vários commits ou se você clonou um
repositório com um histórico de commits pré-existente, você vai
provavelmente querer olhar para trás e ver o que aconteceu. A ferramenta
mais básica e poderosa para fazer isso é o comando git log.

Esses exemplos usam um projeto muito simples chamando "simplegit". Para
conseguir o projeto, execute

\$ git clone https://github.com/schacon/simplegit-progit

Quando você executa git log neste projeto, você deve receber um retorno
que se parece com algo assim:

\$ git log

commit ca82a6dff817ec66f44342007202690a93763949

Author: Scott Chacon \<schacon@gee-mail.com\>

Date: Mon Mar 17 21:52:11 2008 -0700

changed the version number

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7

Author: Scott Chacon \<schacon@gee-mail.com\>

Date: Sat Mar 15 16:40:33 2008 -0700

removed unnecessary test

commit a11bef06a3f659402fe7563abf99ad00de2209e6

Author: Scott Chacon \<schacon@gee-mail.com\>

Date: Sat Mar 15 10:31:28 2008 -0700

first commit

Por padrão, sem argumentos, git log lista os commits feitos neste
repositório em ordem cronológica inversa; isto é, o commit mais recente
aparece primeiro. Como você pode ver, esse comando lista cada commit com
o seu checksum SHA-1, o nome e email do autor, data de inserção, e a
mensagem do commit.

Está disponível um enorme número e variedade de opções para o comando
git log a fim de lhe mostrar exatamente aquilo pelo que está procurando.
Aqui, vamos mostrar a você algumas das mais populares.

Uma das opções que mais ajuda é -p, que mostra as diferenças
introduzidas em cada commit. Você pode também usar -2, que lista no
retorno apenas os dois últimos itens:

\$ git log -p -2

commit ca82a6dff817ec66f44342007202690a93763949

Author: Scott Chacon \<schacon@gee-mail.com\>

Date: Mon Mar 17 21:52:11 2008 -0700

changed the version number

diff \--git a/Rakefile b/Rakefile

index a874b73..8f94139 100644

\-\-- a/Rakefile

+++ b/Rakefile

@@ -5,7 +5,7 @@ require \'rake/gempackagetask\'

spec = Gem::Specification.new do \|s\|

s.platform = Gem::Platform::RUBY

s.name = \"simplegit\"

\- s.version = \"0.1.0\"

\+ s.version = \"0.1.1\"

s.author = \"Scott Chacon\"

s.email = \"schacon@gee-mail.com\"

s.summary = \"A simple gem for using Git in Ruby code.\"

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7

Author: Scott Chacon \<schacon@gee-mail.com\>

Date: Sat Mar 15 16:40:33 2008 -0700

removed unnecessary test

diff \--git a/lib/simplegit.rb b/lib/simplegit.rb

index a0a60ae..47c6340 100644

\-\-- a/lib/simplegit.rb

+++ b/lib/simplegit.rb

@@ -18,8 +18,3 @@ class SimpleGit

end

end

\-

-if \$0 == \_\_FILE\_\_

\- git = SimpleGit.new

\- puts git.show

-end

\\ No newline at end of file

Essa opção mostrar a mesma informação mas com um diff diretamente após
cada item. Isso é de muita ajuda para revisão de código ou para
rapidamente procurar o que aconteceu durante uma série de commits que
uma colaborador tenha adicionado. Você pode também usar uma série de
opções resumidas com o git log. Por exemplo, se você quer ver algumas
estatísticas abreviadas para cada commit, você pode usar a opção
\--stat:

\$ git log \--stat

commit ca82a6dff817ec66f44342007202690a93763949

Author: Scott Chacon \<schacon@gee-mail.com\>

Date: Mon Mar 17 21:52:11 2008 -0700

changed the version number

Rakefile \| 2 +-

1 file changed, 1 insertion(+), 1 deletion(-)

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7

Author: Scott Chacon \<schacon@gee-mail.com\>

Date: Sat Mar 15 16:40:33 2008 -0700

removed unnecessary test

lib/simplegit.rb \| 5 \-\-\-\--

1 file changed, 5 deletions(-)

commit a11bef06a3f659402fe7563abf99ad00de2209e6

Author: Scott Chacon \<schacon@gee-mail.com\>

Date: Sat Mar 15 10:31:28 2008 -0700

first commit

README \| 6 ++++++

Rakefile \| 23 +++++++++++++++++++++++

lib/simplegit.rb \| 25 +++++++++++++++++++++++++

3 files changed, 54 insertions(+)

Como você pode ver, a opção \--stat apresenta abaixo de cada commit uma
lista dos arquivos modificados, quantos arquivos foram modificados, e
quantas linhas nestes arquivos foram adicionadas e removidas. Por último
ela também colocar um resumo das informações.

Uma outra opção realmente muito util é \--pretty. Essa opção modifica os
registros retornados para formar outro formato diferente do padrão.
Algumas opções pré-definidas estão disponíveis para você usar. A opção
oneline mostra cada commit em uma única linha, esta é de muita ajuda se
você está olhando para muitos commits. Em adição, as opções short, full,
e fuller apresentam o retorno quase no mesmo formato porem com menos ou
mais informações, respectivamente:

\$ git log \--pretty=oneline

ca82a6dff817ec66f44342007202690a93763949 changed the version number

085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test

a11bef06a3f659402fe7563abf99ad00de2209e6 first commit

A opção mais interessante é format, a qual permite a você especificar
seu próprio formato de registros de retorno. Isso é especialmente útil
quando você esta gerando um retorno para uma máquina analisar -- pois
você especifica o formato explicitamente, você sabe que isso não irá
mudar com as atualizações do Git:

\$ git log \--pretty=format:\"%h - %an, %ar : %s\"

ca82a6d - Scott Chacon, 6 years ago : changed the version number

085bb3b - Scott Chacon, 6 years ago : removed unnecessary test

a11bef0 - Scott Chacon, 6 years ago : first commit

[[Useful options for]{.underline} [git log
\--pretty=format]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/rpretty_format)
lista algumas das opções mais uteis que format gera.

![](media/image1.png){width="4.322916666666667in" height="7.0625in"}

Table 1. Useful options for git log \--pretty=format

Você talvez esteja imaginando qual a diferença entre *author* e
*committer*. O autor é a pessoa que escreveu originalmente o trabalho,
ao passo que a pessoa que submeteu o trabalho é o committer. Então, se
você criar uma correção para um projeto e um dos membros principais
submete a correção, ambos receberão crédito -- você como autor, e o
membro principal como commiter. Nós vamos abordar esta distinção um
pouco mais em
[[\[ch05-distributed-git\]]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/ch05-distributed-git).

As opções oneline e format são particularmente úteis juntamente com uma
outra opção de log chamada \--graph. Esta opção adiciona um pequeno
gráfico ASCII mostrando seu histórico de branch e merge:

\$ git log \--pretty=format:\"%h %s\" \--graph

\* 2d3acf9 ignore errors from SIGCHLD on trap

\* 5e3ee11 Merge branch \'master\' of git://github.com/dustin/grit

\|\\

\| \* 420eac9 Added a method for getting the current branch.

\* \| 30e367c timeout code and tests

\* \| 5a09431 add timeout protection to grit

\* \| e1193f8 support for heads with slashes in them

\|/

\* d6016bc require time for xmlschema

\* 11d191e Merge branch \'defunkt\' into local

Esse tipo de retorno se tornará mais interessante conforme formos
criando branches e merges no próximo capitulo.

Essas são apenas algumas opções simples de formatações de retorno para
git log -- existem muitas mais. [[Common options to]{.underline} [git
log]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/rlog_options)
lista as opções que nós já abordamos, assim como algumas outras opções
de formatação mais comuns que talvez sejam muito úteis, juntamente com o
como ela mudam o retorno do comando log.

![](media/image2.png){width="6.267716535433071in"
height="4.763888888888889in"}Table 2. Common options to git log

Por exemplo, se você quer ver quais commits estão modificando arquivos
de testes no histórico do código fonte do Git que sofreram commit por
Junio Hamano no mês de Outubro de 2008 e não são commits de merge, você
pode executar algo semelhante a isso:

\$ git log \--pretty=\"%h - %s\" \--author=gitster
\--since=\"2008-10-01\" \\

\--before=\"2008-11-01\" \--no-merges \-- t/

5610e3b - Fix testcase failure when extended attributes are in use

acd3b9e - Enhance hold_lock_file_for\_{update,append}() API

f563754 - demonstrate breakage of detached checkout with symbolic link
HEAD

d1a43f2 - reset \--hard/read-tree \--reset -u: remove unmerged new paths

51a94af - Fix \"checkout \--track -b newbranch\" on detached HEAD

b0ad11e - pull: allow \"git pull origin \$something:\$current_branch\"
into an unborn branch

Dos quase 40.000 commits no histórico do código fonte do Git, esse
comando mostra os 6 que combinam com esses critérios.

## **3.1- Comando Desfazendo**

Em qualquer estágio, você talvez queira desfazer algo. Aqui, vamos rever
algumas ferramentas básicas para desfazer modificações que porventura
tenha feito. Seja cuidadoso, porque nem sempre você pode voltar uma
alteração desfeita. Essa é uma das poucas áreas do Git onde pode perder
algum trabalho feito se você cometer algum engano.

Um dos motivos mais comuns para desfazer um comando, aparece quando você
executa um commit muito cedo e possivelmente esquecendo de adicionar
alguns arquivos ou você escreveu a mensagem do commit de forma
equivocada. Se você quiser refazer este commit, execute o commit
novamente usando a opção \--amend:

\$ git commit \--amend

Esse comando pega a área stage e a usa para realizar o commit. Se você
não fez nenhuma alteração desde o último commit (por exemplo, se você
executar o comando imediatamente depois do commit anterior), então sua
imagem dos arquivos irá ser exatamente a mesma, e tudo o que você
alterará será a mensagem do commit.

O mesmo editor de mensagens de commit é acionado, porém o commit
anterior já possui uma mensagem. Você pode editar a mensagem como
sempre, porém esta sobrescreve a mensagem do commit anterior.

Por exemplo, se você fizer um commit e então lembrar que esqueceu de
colocar no stage as modificações de um arquivo que você quer adicionar
no commit, você pode fazer algo semelhante a isto:

\$ git commit -m \'initial commit\'

\$ git add forgotten_file

\$ git commit \--amend

No final das contas você termina com um único commit -- O segundo commit
substitui o resultante do primeiro.

### **3.3- Retirando um arquivo do Stage**

A próxima sessão demonstra como trabalhar com modificações na área stage
e work directory. A boa notícia é que o comando que você usa para
verificar o estado dessas duas áreas, também te relembra como desfazer
as modificações aplicadas. Por exemplo, vamos supor que você alterou
dois arquivos, e deseja realizar o commit deles separadamente, porém
você acidentalmente digitou git add \* adicionando ambos ao stage. Como
você pode retirar um deles do stage? O comando git status lhe lembrará
de como fazer isso:

\$ git add \*

\$ git status

On branch master

Changes to be committed:

(use \"git reset HEAD \<file\>\...\" to unstage)

renamed: README.md -\> README

modified: CONTRIBUTING.md

Logo abaixo do texto "Changes to be committed", diz git reset HEAD
\<file\>\... para retirar o arquivo do stage. Então, vamos usar esta
sugestão para retirar o arquivo CONTRIBUTING.md do stage:

\$ git reset HEAD CONTRIBUTING.md

Unstaged changes after reset:

M CONTRIBUTING.md

\$ git status

On branch master

Changes to be committed:

(use \"git reset HEAD \<file\>\...\" to unstage)

renamed: README.md -\> README

Changes not staged for commit:

(use \"git add \<file\>\...\" to update what will be committed)

(use \"git checkout \-- \<file\>\...\" to discard changes in working
directory)

modified: CONTRIBUTING.md

O comando é um tanto quanto estranho, mas funciona. O arquivo
CONTRIBUTING.md volta ao estado modificado porem está novamente fora do
stage.

É verdade que o comando git reset pode ser perigoso, especialmente se
você usar a opção \--hard. Entretanto, no cenário descrito acima, o
arquivo no working directory está inalterado, então é relativamente
seguro utilizar o comando.

Essa mágica usando o git reset é tudo que você precisa saber por
enquanto sobre este comando. Nós vamos entra mais no detalhe sobre o que
o comando reset faz e como usá-lo de forma a fazer coisas realmente
interessantes em [[Reset
Demystified]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/r_git_reset).

### **3.4- Desfazendo as Modificações de um Arquivo**

E se você se der conta de que na verdade não quer manter as modificações
do arquivo CONTRIBUTING.md? Como você pode reverter as modificações,
voltando a ser como era quando foi realizado o último commit (ou clone
inicial, ou seja como for que você chegou ao seu working directory)?
Felizmente, git status diz a você como fazer isso também. Neste último
exemplo, a área fora do stage parece com isso:

Changes not staged for commit:

(use \"git add \<file\>\...\" to update what will be committed)

(use \"git checkout \-- \<file\>\...\" to discard changes in working
directory)

modified: CONTRIBUTING.md

Isso lhe diz de forma explicita como descartar as modificações que você
fez. Vamos fazer o que o comando nos sugeriu:

\$ git checkout \-- CONTRIBUTING.md

\$ git status

On branch master

Changes to be committed:

(use \"git reset HEAD \<file\>\...\" to unstage)

renamed: README.md -\> README

Você pode notar que as modificações foram revertidas.

É importante entender que o git checkout \-- \<file\> é um a comando
perigoso. Qualquer modificação que você fez no arquivo se foi --- O Git
apenas substitui o arquivo pela última versão (mais recente) que sofreu
commit. Nunca use este comando a não ser que você saiba com certeza que
não quer salvar as modificações do arquivo.

Se você gostaria de manter as modificações que fez no arquivo, porem
precisa tirá-lo do caminho por enquanto, sugerimos que pule para a
documentação sobre
Branches[[\[ch03-git-branching\]]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/ch03-git-branching);
esta geralmente é a melhor forma de fazer isso.

Lembre-se, qualquer coisa que sobre commit com Git pode quase sempre ser
recuperada. Até mesmo commits que estava em algum branches que foram
deletados ou commits que forma sobre escritos através de um \--amend
podem ser recuperados (veja[[Data
Recovery]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/r_data_recovery)
para recuperação de dados). Contudo, qualquer coisa que você perder que
nunca sofreu commit pode ser considerada praticamente perdida.

## **4- Trabalhando de Forma Remota**

Para colaborar com qualquer projeto Git, você precisará saber como
gerenciar seus repositórios remotos. Repositórios remotos são versões de
seu repositório hospedado na Internet ou em uma rede qualquer. Você pode
ter vários deles, cada um dos quais geralmente é ou somente leitura ou
leitura/escrita. Colaborar com outras pessoas envolve o gerenciamento
destes repositórios remotos, fazer *pushing*(atualizar) e
*pulling*(obter) de dados para e deles quando você precisar compartilhar
seu trabalho. Gerenciar repositórios remotos inclui saber como
adicioná-los remotamente, remover aqueles que não são mais válidos,
gerenciar vários *branches*(ramos) e definí-los como rastreados ou não e
muito mais. Nesta seção, abordaremos algumas destas habilidades de
gereciamento remoto.

### **4.1- Exibindo seus repositórios remotos**

Para ver quais servidores remotos você configurou, você pode executar o
comando git remote. Ele lista os nomes abreviados de cada repositório
remoto manejado que você especificou. Se você clonou seu repositório,
você deve pelo menos ver *origin*(origem) -- que é o nome padrão dado
pelo Git ao servidor que você clonou:

\$ git clone https://github.com/schacon/ticgit

Cloning into \'ticgit\'\...

remote: Reusing existing pack: 1857, done.

remote: Total 1857 (delta 0), reused 0 (delta 0)

Receiving objects: 100% (1857/1857), 374.35 KiB \| 268.00 KiB/s, done.

Resolving deltas: 100% (772/772), done.

Checking connectivity\... done.

\$ cd ticgit

\$ git remote

origin

Você também pode especificar -v, que mostra as URLs que o Git tem
armazenado pelo nome abreviado a ser usado para ler ou gravar naquele
repositório remoto:

\$ git remote -v

origin https://github.com/schacon/ticgit (fetch)

origin https://github.com/schacon/ticgit (push)

Se você tem mais de um repositório remoto, o comando lista todos eles.
Por exemplo, um repositório com diversos repositórios remotos para
trabalhar com vários colaboradores pode ser algo parecido com isto:

\$ cd grit

\$ git remote -v

bakkdoor https://github.com/bakkdoor/grit (fetch)

bakkdoor https://github.com/bakkdoor/grit (push)

cho45 https://github.com/cho45/grit (fetch)

cho45 https://github.com/cho45/grit (push)

defunkt https://github.com/defunkt/grit (fetch)

defunkt https://github.com/defunkt/grit (push)

koke git://github.com/koke/grit.git (fetch)

koke git://github.com/koke/grit.git (push)

origin git@github.com:mojombo/grit.git (fetch)

origin git@github.com:mojombo/grit.git (push)

Isto significa que nós podemos obter(*pull*) contribuições de qualquer
um desses usuários muito facilmente. Nós podemos, adicionalmente, ter a
permissão de atualizar(*push*) um ou mais destes, embora não possamos
dizer isso nesse caso.

Note que estes repositórios remotos usam uma variedade de protocolos e
nós falaremos mais sobre isso em [[Getting Git on a
Server]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/r_git_on_the_server).

### **4.2- Adicionando Repositórios Remotos**

Nós mencionamos e demos algumas demonstrações de como o comando clone
implicitamente adiciona a origem(origin) remota para você. Aqui está
como adicionar um novo repositório remoto explicitamente. Para adicionar
um novo repositório Git remoto como um nome curto que você pode
referenciar facilmente, execute git remote add \<shortname\> \<url\>:

\$ git remote

origin

\$ git remote add pb https://github.com/paulboone/ticgit

\$ git remote -v

origin https://github.com/schacon/ticgit (fetch)

origin https://github.com/schacon/ticgit (push)

pb https://github.com/paulboone/ticgit (fetch)

pb https://github.com/paulboone/ticgit (push)

Agora você pode usar a string pb na linha de comando no lugar de uma URL
completa. Por exemplo, se você quiser buscar toda a informação que Paul
tem, mas você ainda não tem em seu repositório, você pode executar git
fetch pb:

\$ git fetch pb

remote: Counting objects: 43, done.

remote: Compressing objects: 100% (36/36), done.

remote: Total 43 (delta 10), reused 31 (delta 5)

Unpacking objects: 100% (43/43), done.

From https://github.com/paulboone/ticgit

\* \[new branch\] master -\> pb/master

\* \[new branch\] ticgit -\> pb/ticgit

O *master branch*(ramo mestre) do Paul agora está acessível localmente
como pb/master -- você pode fundí-lo(*merge*) dentro de uma de suas
ramificações(*branches*) ou você pode checar fora da ramificação local
se você quiser inspecioná-lo. (Nós abordaremos o que são
ramificações(*branches*) e como usá-las mais detalhadamente em
[[\[ch03-git-branching\]]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/ch03-git-branching).
)

### **4.3- Buscando e Obtendo de seus Repositórios Remotos**

Como você viu, para obter dados de seus projetos remotos, você pode
executar:

\$ git fetch \[remote-name\]

O comando vai até aquele projeto remoto e extrai todos os dados daquele
projeto que você ainda não tem. Depois que você faz isso, você deve ter
como referência todos as ramificações(*branches*) daquele repositório
remoto, que você pode mesclar(*merge*) com o atual ou inspecionar a
qualquer momento.

Se você clonar um repositório, o comando automaticamente adiciona àquele
repositório remoto com o nome origin. Então, git fetch origin busca
qualquer novo trabalho que tenha sido enviado para aquele servidor desde
que você o clonou ou fez a última busca(*fetch*). É importante notar que
o comando git fetch só baixa os dados para o seu repositório local - ele
não é automaticamente mesclado(*merge*) com nenhum trabalho seu ou
modificação que você esteja trabalhando atualmente. Você deve mesclá-los
manualmente dentro de seu trabalho quando você estiver pronto.

Se o branch atual é configurando para rastrear um branch remoto (veja a
próxima seção e
[[\[ch03-git-branching\]]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/ch03-git-branching)
para mais informação), você pode usar o comando git pull para
buscar(*fetch*) e então mesclar(*merge*) automaticamente aquele branch
remoto dentro do seu branch atual. Este pode ser um fluxo de trabalho
mais fácil e mais confortável para você, e por padrão, o comando git
clone automaticamente configura a sua master branch local para rastrear
a master branch remota ou qualquer que seja o nome do branch padrão no
servidor de onde você o clonou. Executar git pull comumente busca os
dados do servidor de onde você originalmente clonou e automaticamente
tenta mesclá-lo dentro do código que você está atualmente trabalhando.

### **4.4- Pushing to Your Remotes**

Quando você tem seu projeto em um ponto que deseja compartilhar, é
necessário enviá-lo para o servidor remoto. O comando para isso é
simples: git push \[remote-name\] \[branch-name\]. Se você quiser enviar
sua ramificacão (branch) master para o servidor origin (novamente, a
clonagem geralmente configura ambos os nomes para você automaticamente),
então você pode executar isso para enviar quaisquer commits feitos para
o servidor:

\$ git push origin master

Este comando funciona apenas se você clonou de um servidor ao qual você
tem acesso de escrita (write-access) e se ninguém mais utilizou o
comando push nesse meio-tempo. Se você e outra pessoa clonarem o
repositório ao mesmo tempo e ela utilizar o comando push e, em seguida,
você tentar utilizar, seu envio será rejeitado. Primeiro você terá que
atualizar localmente, incorporando o trabalho dela ao seu, só assim você
poderá utilizar o comando push. Veja
[[\[ch03-git-branching\]]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/ch03-git-branching)
para informações mais detalhadas sobre como enviar para servidores
remotos.

### **4.5- Inspecionando o Servidor Remoto**

Se você quiser ver mais informações sobre um servidor remoto em
particular, você pode usar o comando git remote show \[nome-remoto\]. Ao
executar este comando com um nome abreviado específico, como origin,
obterá algo assim:

\$ git remote show origin

\* remote origin

Fetch URL: https://github.com/schacon/ticgit

Push URL: https://github.com/schacon/ticgit

HEAD branch: master

Remote branches:

master tracked

dev-branch tracked

Local branch configured for \'git pull\':

master merges with remote master

Local ref configured for \'git push\':

master pushes to master (up to date)

Ele lista a URL para o repositório remoto, bem como as informações de
rastreamento do branch. O comando, de forma útil, comunica que se você
estiver no branch master e executar git pull, ele irá mesclar (merge)
automaticamente no branch master do servidor após buscar (fetch) todas
as referências remotas. Ele também lista todas as referências remotas
recebidas.

Esse é um exemplo simples que você provavelmente encontrará. Quando você
usa o Git mais intensamente, no entanto, pode ver muito mais informações
com git remote show:

\$ git remote show origin

\* remote origin

URL: https://github.com/my-org/complex-project

Fetch URL: https://github.com/my-org/complex-project

Push URL: https://github.com/my-org/complex-project

HEAD branch: master

Remote branches:

master tracked

dev-branch tracked

markdown-strip tracked

issue-43 new (next fetch will store in remotes/origin)

issue-45 new (next fetch will store in remotes/origin)

refs/remotes/origin/issue-11 stale (use \'git remote prune\' to remove)

Local branches configured for \'git pull\':

dev-branch merges with remote dev-branch

master merges with remote master

Local refs configured for \'git push\':

dev-branch pushes to dev-branch (up to date)

markdown-strip pushes to markdown-strip (up to date)

master pushes to master (up to date)

Este comando mostra para qual ramificação (branch) é enviada
automaticamente quando você executa git push enquanto em certas
ramificações. Ele também mostra quais branches remotos do servidor você
ainda não tem, quais você tem que foram removidos do servidor e várias
branches locais que são capazes de se fundir automaticamente com seu
branch de rastreamento remoto quando você executa git pull.

### **4.6- Removendo e Renomeando Remotes**

Você pode utilizar o git remote rename para alterar o nome curto de
servidores remotos. Por exemplo, se você deseja renomear pb para\`
paul\`, você pode fazer isso com git remote rename:

\$ git remote rename pb paul

\$ git remote

origin

paul

Vale a pena mencionar que isso muda todos os nomes de ramificações de
rastreamento remoto também. O que costumava ser referenciado em
pb/master agora está em paul/master.

Se você quiser remover um servidor remoto por algum motivo - e você
anteriormente moveu o servidor ou não está mais usando um em particular,
ou talvez um contribuidor não esteja mais contribuindo - você pode usar
git remote remove ou git remote rm:

\$ git remote remove paul

\$ git remote

origin

## **5- Criando Tags**

Da mesma forma que a maioria dos VCSs, o Git tem a habilidade de marcar
pontos específicos na história como sendo importantes. Normalmente as
pessoas usam essa funcionalidade para marcar pontos onde foram feitas
releases (v1.0 e assim por diante). Nessa sessão, você irá aprender como
listar as tags existentes, como criar novas tags e quais são os
diferentes tipos de tags.

### **5.1- Listando suas Tags**

Listar tags disponíveis no Git não tem rodeios. Apenas escreva git tag:

\$ git tag

v0.1

v1.3

Esse comando lista as tags em ordem alfabética; a ordem na qual eles
aparecerem não tem real importância.

Você pode também procurar por tags com algum padrão em especifico. O
repositório fonte do Git, por exemplo, contém mais de 500 tags. Se você
deseja procurar apenas a série 1.8.5, você pode executar isso:

\$ git tag -l \"v1.8.5\*\"

v1.8.5

v1.8.5-rc0

v1.8.5-rc1

v1.8.5-rc2

v1.8.5-rc3

v1.8.5.1

v1.8.5.2

v1.8.5.3

v1.8.5.4

v1.8.5.5

### **5.2- Criando Tags**

O Git usa dois tipos de tags: Leve e Anotada.

Uma tag do tipo leve é muito parecida com um branch que não muda -- Ela
apenas aponta para um commit em especifico.

Tags anotadas, entretanto, são um armazenamento completo de objetos no
banco de dados do Git. Elas têm checksum, contem marcações de nome,
email e data; têm uma mensagem de tag; e podem ser assinadas e
asseguradas pela GPG (GNU Privacy Guard). É geralmente recomendado que
você crie tags anotadas assim você tem todas as informações; mas se você
quer um tag temporária ou por alguma razão não quer manter todas as
informações, tags do tipo leve esta disponíveis para isso.

### **5.3- Tags Anotadas**

Criar uma tag anotada no Git é simples. A forma mais fácil é por
especificar o parâmetro -a quando você executa o comando tag:

\$ git tag -a v1.4 -m \"my version 1.4\"

\$ git tag

v0.1

v1.3

v1.4

O -m define a mensagem de tag, a qual é armazenada junto com a tag. Se
você não especificar uma mensagem para uma tag anotada, o Git abre seu
editor para que você possa digitar nele.

Você pode ver os dados da tag juntamente com o commit onde ela foi
realizada usando o comando git show:

\$ git show v1.4

tag v1.4

Tagger: Ben Straub \<ben@straub.cc\>

Date: Sat May 3 20:19:12 2014 -0700

my version 1.4

commit ca82a6dff817ec66f44342007202690a93763949

Author: Scott Chacon \<schacon@gee-mail.com\>

Date: Mon Mar 17 21:52:11 2008 -0700

changed the version number

Este mostra as informações de tag, a data do commit atrelado a tag, e a
mensagem antes mesmo de mostrar as informações do commit.

### **5.4- Tags de Tipo Leve**

Uma outra forma de submeter uma tag é usando o tipo leve. Isso é
basicamente o checksum de commit armazenando em um arquivo -- nenhuma
outra informação é mantida. Para criar uma tag do tipo leve, não forneça
as opções -a, -s, or -m:

\$ git tag v1.4-lw

\$ git tag

v0.1

v1.3

v1.4

v1.4-lw

v1.5

Dessa vez, se você executar git show nessa tag, você não verá as
informações extras. O comando apenas mostrará o commit:

\$ git show v1.4-lw

commit ca82a6dff817ec66f44342007202690a93763949

Author: Scott Chacon \<schacon@gee-mail.com\>

Date: Mon Mar 17 21:52:11 2008 -0700

changed the version number

### **5.5- Criando Tags posteriormente**

Você pode também criar uma tag após já ter realizado o commit. Suponha
que seu histórico de commits seja semelhante a este:

\$ git log \--pretty=oneline

15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch \'experiment\'

a6b4c97498bd301d84096da251c98a07c7723e65 beginning write support

0d52aaab4479697da7686c15f77a3d64d9165190 one more thing

6d52a271eda8725415634dd79daabbc4d9b6008e Merge branch \'experiment\'

0b7434d86859cc7b8c3d5e1dddfed66ff742fcbc added a commit function

4682c3261057305bdd616e23b64b0857d832627b added a todo file

166ae0c4d3f420721acbb115cc33848dfcc2121a started write support

9fceb02d0ae598e95dc970b74767f19372d61af8 updated rakefile

964f16d36dfccde844893cac5b347e7b3d44abbc commit the todo

8a5cbc430f1a9c3d00faaeffd07798508422908a updated readme

Agora, suponha que você esqueceu de criar a tag na versão v1.2, a qual
equivale ao commit "updated rakefile". Você pode adicionar isso
posteriormente. Para criar uma tag neste commit, você deve informar o
checksum do commit (ou parte dele) ao final do comando:

\$ git tag -a v1.2 9fceb02

Você pode ver que criou uma tag para o commit:

\$ git tag

v0.1

v1.2

v1.3

v1.4

v1.4-lw

v1.5

\$ git show v1.2

tag v1.2

Tagger: Scott Chacon \<schacon@gee-mail.com\>

Date: Mon Feb 9 15:32:16 2009 -0800

version 1.2

commit 9fceb02d0ae598e95dc970b74767f19372d61af8

Author: Magnus Chacon \<mchacon@gee-mail.com\>

Date: Sun Apr 27 20:43:35 2008 -0700

updated rakefile

\...

### **5.6- Compartilhando Tags**

Por padrão, o comando git push não envia as tags para os servidores
remoto. Você terá que explicitamente enviar as tags para o servidor de
compartilhamento depois de tê-las criado. Esse processo é semelhante a
compartilhar branches remotos -- você pode executar git push origin
\[tagname\].

\$ git push origin v1.5

Counting objects: 14, done.

Delta compression using up to 8 threads.

Compressing objects: 100% (12/12), done.

Writing objects: 100% (14/14), 2.05 KiB \| 0 bytes/s, done.

Total 14 (delta 3), reused 0 (delta 0)

To git@github.com:schacon/simplegit.git

\* \[new tag\] v1.5 -\> v1.5

Se você tem muitas tags que você quer enviar de uma vez, você pode
também usar a opção \--tags atrelada ao comando git push. Isso vai
transferir todas as suas tags ainda não enviadas para o servidor remoto.

\$ git push origin \--tags

Counting objects: 1, done.

Writing objects: 100% (1/1), 160 bytes \| 0 bytes/s, done.

Total 1 (delta 0), reused 0 (delta 0)

To git@github.com:schacon/simplegit.git

\* \[new tag\] v1.4 -\> v1.4

\* \[new tag\] v1.4-lw -\> v1.4-lw

Agora, quando alguém executar um clone ou pull a partir do seu
repositório, ele vai puxar também todas as tags. ==== Checando as Tags

Você não pode realizar o checkout de uma tag no Git, uma vez que elas
não podem ser movidas. Se você quer deixar uma versão do seu repositório
idêntica a uma tag especifica em seu diretório de trabalho, você pode
criar um novo branch em uma tag especifica com o comando git checkout -b
\[branchname\] \[tagname\]:

\$ git checkout -b version2 v2.0.0

Switched to a new branch \'version2\'

Claro que se você fazer isso e realizar um commit, seu branch version2
será ligeiramente diferente do que a tag v2.0.0 assim você irá seguir em
frente com suas novas modificações, então seja cuidadoso.
