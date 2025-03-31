**1- ntrodução**

Quase todo Sistema de Controle de Versionamento tem alguma forma de
suporte a ramificações (Branches). Ramificação significa que você
diverge da linha principal de desenvolvimento e continua a trabalhar sem
alterar essa linha principal. Em muitas ferramentas de versionamento,
este é um processo um tanto difícil, geralmente exigindo que você crie
uma nova cópia do diretório do código-fonte, o que pode demorar muito em
projetos maiores.

Algumas pessoas se referem ao modelo de ramificação do Git como seu
"recurso matador" e certamente diferencia o Git na comunidade de
sistemas de versionamento. Por que isso é tão especial? A forma como o
Git cria branches é incrivelmente leve, tornando as operações de
ramificação quase instantâneas, alternando entre os branches geralmente
com a mesma rapidez. Ao contrário de muitos outros sistemas, o Git
incentiva fluxos de trabalho que se ramificam e se fundem com
frequência, até mesmo várias vezes ao dia. Compreender e dominar esse
recurso oferece uma ferramenta poderosa e única e pode mudar totalmente
a maneira como você desenvolve.

## **2- O que são Branches?**

Para realmente entender como o Git trabalha com Branches, precisamos dar
um passo atrás e examinar como o Git armazena seus dados.

Como você deve se lembrar de \<\< ch01-introdução \>\>, o Git não
armazena dados como uma série de mudanças ou diferenças, mas sim como
uma série de snapshots (instantâneos de um momento) .

Quando você faz um commit, o Git armazena um objeto de commit que contém
um ponteiro para o snapshot do conteúdo que você testou. Este objeto
também contém o nome do autor e o e-mail, a mensagem que você digitou e
ponteiros para o commit ou commits que vieram antes desse commit (seu
pai ou pais): sem pai para o commit inicial, um pai para um commit
normal, e vários pais para um commit que resulta de uma fusão de dois ou
mais branches.

Para verificar isso, vamos assumir que você tem um diretório contendo
três arquivos, e você seleciona todos eles e efetua o commit. Ele
Prepara os arquivos e calcula uma verificação para cada um (o hash SHA-1
que mencionamos em \<\< ch01-introdução \>\>), armazena essa versão do
arquivo no repositório Git (Git se refere a eles como blobs), e adiciona
esse hash de verificação à área de preparação (staging area):

\$ git add README test.rb LICENSE

\$ git commit -m \'The initial commit of my project\'

Quando você faz um commit executando git commit, o Git verifica cada
subdiretório (neste caso, apenas o diretório raiz do projeto) e armazena
esses objetos no repositório do Git. O Git então cria um objeto de
commit que possui os metadados e um ponteiro para a raiz do projeto para
que ele possa recriar aquele snapshot quando necessário.

Seu repositório Git agora contém cinco objetos: um blob para o conteúdo
de cada um dos seus três arquivos, uma árvore que lista o conteúdo do
diretório e especifica quais nomes de arquivo são armazenados e quais
seus blobs e um commit com o ponteiro para essa árvore e todos os
metadados de commit.

![](media/image31.png){width="6.267716535433071in"
height="3.4722222222222223in"}

igure 9. Um commit e sua árvore

Se você fizer algumas mudanças e confirmar novamente, o próximo commit
armazena um ponteiro para o commit que veio imediatamente antes dele.

![](media/image20.png){width="6.267716535433071in"
height="2.0694444444444446in"}

![](media/image27.png){width="6.267716535433071in"
height="3.361111111111111in"}

Figure 11. Um branch e seu histórico de commits

### **3- Criando um Novo Branch**

O que acontece se você criar um novo branch? Bem, fazer isso cria um
novo ponteiro para você mover. Digamos que você crie um novo branch
chamado: testing. Você faz isso com o comando git branch :

\$ git branch testing

Isso cria um novo ponteiro para o mesmo commit em que você está
atualmente.

![](media/image26.png){width="6.267716535433071in"
height="2.4444444444444446in"}

Figure 12. Duas branches apontando para a mesma série de commits

Como o Git sabe em qual branch você está atualmente? Ele mantém um
ponteiro especial chamado HEAD. Note que isso é muito diferente do
conceito de HEAD em outros sistemas de versionamento com os quais você
pode estar acostumado, como Subversion ou CVS. No Git, isso é um
ponteiro para o branch local em que você está. Neste caso, você ainda
está em master. O comando git branch apenas *criou* um novo branch - ele
não mudou para aquele branch.

![](media/image30.png){width="6.267716535433071in"
height="3.4027777777777777in"}

Figure 14. HEAD aponta para o branch atual

O que isso significa? Bem, vamos fazer outro commit:

\$ vim test.rb

\$ git commit -a -m \'made a change\'

![](media/image28.png){width="6.267716535433071in"
height="2.513888888888889in"}

Figure 15. O branch do HEAD avança quando um commit é feito

Isso é interessante, porque agora seu branch testing avançou, mas seu
branch master ainda aponta para o commit em que você estava quando
executou git checkout para alternar entre os branches. Vamos voltar para
o branch master:

\$ git checkout master

![](media/image29.png){width="6.267716535433071in"
height="2.513888888888889in"}

Figure 16. O HEAD se move quando você faz o checkout

Esse comando fez duas coisas. Ele moveu o ponteiro HEAD de volta para
apontar para o branch master, e reverteu os arquivos em seu diretório de
trabalho de volta para o snapshot para o qual master aponta. Isso também
significa que as alterações feitas a partir deste ponto irão divergir de
uma versão mais antiga do projeto. Essencialmente, ele retrocede o
trabalho que você fez em seu branch testing para que você possa ir em
uma direção diferente.

Exemplo:

\$ vim test.rb

\$ git commit -a -m \'made other changes\'

Agora o histórico do seu projeto divergiu (consulte [[Histórico de
diferenças]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/rdivergent_history)).
Você criou e mudou para um branch, fez algum trabalho nele e, em
seguida, voltou para o seu branch principal e fez outro trabalho. Ambas
as mudanças são isoladas em branches separados: você pode alternar entre
os branches e mesclá-los quando estiver pronto. E você fez tudo isso com
comandos simples branch, checkout e commit.

![](media/image23.png){width="6.267716535433071in"
height="4.013888888888889in"}

Figure 17. Histórico de diferenças

Você também pode ver isso facilmente com o comando git log. Se você
executar git log \--oneline \--decorate \--graph \--all, ele mostrará o
histórico de seus commits, exibindo onde estão seus ponteiros de branch
e como seu histórico divergiu.

\$ git log \--oneline \--decorate \--graph \--all

\* c2b9e (HEAD, master) made other changes

\| \* 87ab2 (testing) made a change

\|/

\* f30ab add feature #32 - ability to add new formats to the

\* 34ac2 fixed bug #1328 - stack overflow under certain conditions

\* 98ca9 initial commit of my project

Como um branch no Git é na verdade um arquivo simples que contém a
verificação SHA-1 de 40 caracteres do commit para o qual ele aponta,
branches são fáceis de criar e destruir. Criar um novo branch é tão
rápido e simples quanto escrever 41 bytes em um arquivo (40 caracteres e
uma nova linha).

Isso está em nítido contraste com a forma como as ferramentas de
versionamento mais antigas se ramificam, o que envolve a cópia de todos
os arquivos do projeto em um segundo diretório. Isso pode levar vários
segundos ou até minutos, dependendo do tamanho do projeto, enquanto no
Git o processo é sempre instantâneo. Além disso, como estamos gravando
os pais quando fazemos o commit, encontrar uma base adequada para a
mesclagem é feito automaticamente para nós e geralmente é muito fácil de
fazer. Esses recursos ajudam a incentivar os desenvolvedores a criar e
usar branches com frequência.

# **4- O básico de Ramificação (Branch) e Mesclagem (Merge)**

Vamos ver um exemplo simples de ramificação (*branching*) e mesclagem
(*merging*) com um fluxo de trabalho que você pode vir a usar no mundo
real. Você seguirá os seguintes passos:

1.  Trabalhar um pouco em um website.

2.  Criar um *branch* para um nova história de usuário na qual você está
    trabalhando.

3.  Trabalhar um pouco neste novo *branch*.

Nesse ponto, você vai receber uma mensagem dizendo que outro problema é
crítico e você precisa fazer a correção. Você fará o seguinte:

1.  Mudar para o seu branch de produção.

2.  Criar um novo branch para fazer a correção.

3.  Após testar, fazer o merge do branch de correção, e fazer push para
    produção.

4.  Voltar para sua história de usuário original e continuar
    trabalhando.

### **5- Ramificação Básica**

Primeiramente, digamos que você esteja trabalhando em seu projeto e já
tenha alguns commits no branch master.

![](media/image25.png){width="6.267716535433071in"
height="1.7222222222222223in"}

Figure 18. Um histórico de commits simples

Você decidiu que você vai trabalhar no chamado #53 em qualquer que seja
o sistema de gerenciamento de chamados que a sua empresa usa.

Para criar um novo branch e mudar para ele ao mesmo tempo, você pode
executar o comando git checkout com o parâmetro -b:

\$ git checkout -b iss53

Switched to a new branch \"iss53\"

Esta é a abreviação de:

\$ git branch iss53

\$ git checkout iss53

![](media/image24.png){width="6.267716535433071in"
height="2.9027777777777777in"}Figure 19. Criando um novo ponteiro de
branch

Você trabalha no seu website e adiciona alguns commits.

Ao fazer isso, você move o branch iss53 para a frente, pois este é o
branch que está selecionado, ou *checked out*(isto é, seu HEAD está
apontando para ele):

\$ vim index.html

\$ git commit -a -m \'Create new footer \[issue 53\]\'

Figure 20. O branch iss53 moveu para a frente graças ao seu trabalho

Agora você recebe a ligação dizendo que há um problema com o site, e que
você precisa corrigí-lo imediatamente. Com o Git, você não precisa
enviar sua correção junto com as alterações do branch iss53 que já fez.
Você também não precisa se esforçar muito para desfazer essas alterações
antes de poder trabalhar na correção do erro em produção. Tudo o que
você precisa fazer é voltar para o seu branch master.

Entretanto, antes de fazer isso, note que se seu diretório de trabalho
ou stage possui alterações ainda não commitadas que conflitam com o
branch que você quer usar, o Git não deixará que você troque de branch.
O melhor é que seu estado de trabalho atual esteja limpo antes de trocar
de branches. Há maneiras de contornar isso (a saber, o comando stash e
commit com a opção \--ammend) que iremos cobrir mais tarde, em
[[Stashing and
Cleaning]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/r_git_stashing).
Por agora, vamos considerar que você fez commit de todas as suas
alterações, de forma que você pode voltar para o branch master:

\$ git checkout master

Switched to branch \'master\'

Neste ponto, o diretório de trabalho de seu projeto está exatamente da
forma como estava antes de você começar a trabalhar no chamado #53, e
você pode se concentrar na correção. Isso é importante de se ter em
mente: quando você troca de branches, o Git reseta seu diretório de
trabalho para a forma que era na última vez que você commitou naquele
branch. O Git adiciona, remove e modifica arquivos automaticamente para
se assegurar que a sua cópia de trabalho seja igual ao estado do branch
após você adicionar o último commit a ele.

Seu próximo passo é fazer a correção necessária; Vamos criar um branch
chamado hotfix no qual trabalharemos até a correção estar pronta:

\$ git checkout -b hotfix

Switched to a new branch \'hotfix\'

\$ vim index.html

\$ git commit -a -m \'Fix broken email address\'

\[hotfix 1fb7853\] Fix broken email address

1 file changed, 2 insertions(+)

![](media/image33.png){width="6.267716535433071in" height="3.0in"}

Figure 21. Branch de correção (hotfix) baseado em master

Você pode executar seus testes, se assegurar que a correção está do
jeito que você quer, e finalmente mesclar o branch hotfix de volta para
o branch master para poder enviar para produção. Para isso, você usa o
comando git merge command:

\$ git checkout master

\$ git merge hotfix

Updating f42c576..3a0874c

Fast-forward

index.html \| 2 ++

1 file changed, 2 insertions(+)

Você vai notar a expressão "fast-forward" nesse merge. Já que o branch
hotfix que você mesclou aponta para o commit C4 que está diretamente à
frente do commit C2 no qual você está agora, o Git simplesmente move o
ponteiro para a frente. Em outras palavras, quando você tenta mesclar um
commit com outro commit que pode ser alcançado por meio do histórico do
primeiro commit, o Git simplifica as coisas e apenas move o ponteiro
para a frente porque não há nenhum alteração divergente para
mesclar --- isso é conhecido como um merge "fast-forward."

Agora, a sua alteração está no snapshot do commmit para o qual o branch
master aponta, e você pode enviar a correção.

![](media/image32.png){width="6.267716535433071in"
height="3.763888888888889in"}

Figure 22. o branch master sofre um \"fast-forward\" até hotfix

Assim que a sua correção importantíssima é entregue, você já pode voltar
para o trabalho que estava fazendo antes da interrupção. Porém, você irá
antes excluir o branch hotfix, pois ele já não é mais necessário --- o
branch master aponta para o mesmo lugar. Você pode remover o branch
usando a opção -d com o comando git branch:

\$ git branch -d hotfix

Deleted branch hotfix (3a0874c).

Agora você pode retornar ao branch com seu trabalho em progresso na
*issue* #53 e continuar trabalhando.

\$ git checkout iss53

Switched to branch \"iss53\"

\$ vim index.html

\$ git commit -a -m \'Finish the new footer \[issue 53\]\'

\[iss53 ad82d7a\] Finish the new footer \[issue 53\]

1 file changed, 1 insertion(+)

![](media/image36.png){width="6.267716535433071in"
height="2.9166666666666665in"}

Figure 23. Continuando o trabalho no branch iss53

É importante frisar que o trabalho que você fez no seu branch hotfix não
está contido nos arquivos do seu branch iss53. Caso você precise dessas
alterações, você pode fazer o merge do branch master no branch iss53
executando git merge master, ou você pode esperar para integrar essas
alterações até que você decida mesclar o branch iss53 de volta para
master mais tarde.

### **6- Mesclagem Básica**

Digamos que você decidiu que o seu trabalho no chamado #53 está completo
e pronto para ser mesclado de volta para o branch master. Para fazer
isso, você precisa fazer o merge do branch iss53, da mesma forma com que
você mesclou o branch hotfix anteriormente. Tudo o que você precisa
fazer é mudar para o branch que receberá as alterações e executar o
comando git merge:

\$ git checkout master

Switched to branch \'master\'

\$ git merge iss53

Merge made by the \'recursive\' strategy.

index.html \| 1 +

1 file changed, 1 insertion(+)

Isso é um pouco diferente do merge anterior que você fez com o branch
hotfix. Neste caso, o histórico de desenvolvimento divergiu de um ponto
mais antigo. O Git precisa trabalhar um pouco mais, devido ao fato de
que o commit no seu branch atual não é um ancestral direto do branch
cujas alterações você quer integrar. Neste caso, o Git faz uma simples
mesclagem de três vias (*three-way merge*), usando os dois snapshots
referenciados pela ponta de cada branch e o ancestral em comum dos dois.

![](media/image37.png){width="6.267716535433071in"
height="2.9027777777777777in"}

Figure 24. Três snapshots usados em um merge típico

Ao invés de apenas mover o ponteiro do branch para a frente, o Git cria
um novo snapshot que resulta desse merge em três vias e automaticamente
cria um novo commit que aponta para este snapshot. Esse tipo de commit é
chamado de commit de merge, e é especial porque tem mais de um pai.

![Um commit de merge.](media/image18.png){width="6.267716535433071in"
height="2.4166666666666665in"}

Figure 25. Um commit de merge

Agora que seu trabalho foi integrado, você não precisa mais do branch
iss53. Você pode encerrar o chamado no seu sistema e excluir o branch:

\$ git branch -d iss53

### **7- Conflitos Básicos de Merge**

De vez em quando, esse processo não acontece de maneira tão tranquila.
Se você mudou a mesma parte do mesmo arquivo de maneiras diferentes nos
dois branches que você está tentando mesclar, o Git não vai conseguir
integrá-los de maneira limpa. Se a sua correção para o problema #53
modificou a mesma parte de um arquivo que também foi modificado em
hotfix, você vai ter um conflito de merge que se parece com isso:

\$ git merge iss53

Auto-merging index.html

CONFLICT (content): Merge conflict in index.html

Automatic merge failed; fix conflicts and then commit the result.

O Git não criou automaticamente um novo commit de merge. Ele pausou o
processo enquanto você soluciona o conflito. Para ver quais arquivos não
foram mesclados a qualquer momento durante um conflito de merge, você
pode executar git status:

\$ git status

On branch master

You have unmerged paths.

(fix conflicts and run \"git commit\")

Unmerged paths:

(use \"git add \<file\>\...\" to mark resolution)

both modified: index.html

no changes added to commit (use \"git add\" and/or \"git commit -a\")

Qualquer arquivo que tenha conflitos que não foram solucionados é
listado como *unmerged*(\"não mesclado\"). O Git adiciona símbolos
padrão de resolução de conflitos nos arquivos que têm conflitos, para
que você possa abrí-los manualmente e solucionar os conflitos. O seu
arquivo contém uma seção que se parece com isso:

\<\<\<\<\<\<\< HEAD:index.html

\<div id=\"footer\"\>contact : email.support@github.com\</div\>

=======

\<div id=\"footer\"\>

please contact us at support@github.com

\</div\>

\>\>\>\>\>\>\> iss53:index.html

Isso significa que a versão em HEAD (o seu branch master, porque era o
que estava selecionado quando você executou o comando merge) é a parte
superior daquele bloco (tudo acima de =======), enquanto que a versão no
branch iss53 contém a versão na parte de baixo. Para solucionar o
conflito, você precisa escolher um dos lados ou mesclar os conteúdos
diretamente. Por exemplo, você pode resolver o conflito substituindo o
bloco completo por isso:

\<div id=\"footer\"\>

please contact us at email.support@github.com

\</div\>

Essa solução tem um pouco de cada versão, e as linhas com os símbolos
\<\<\<\<\<\<\<, =======, e \>\>\>\>\>\>\> foram completamente removidas.
Após solucionar cada uma dessas seções em cada arquivo com conflito,
execute git add em cada arquivo para marcá-lo como solucionado.
Adicionar o arquivo ao stage o marca como resolvido para o Git.

Se você quiser usar uma ferramenta gráfica para resolver os conflitos,
você pode executar git mergetool, que inicia uma ferramenta de mesclagem
visual apropriada e guia você através dos conflitos:

\$ git mergetool

This message is displayed because \'merge.tool\' is not configured.

See \'git mergetool \--tool-help\' or \'git help config\' for more
details.

\'git mergetool\' will now attempt to use one of the following tools:

opendiff kdiff3 tkdiff xxdiff meld tortoisemerge gvimdiff diffuse
diffmerge ecmerge p4merge araxis bc3 codecompare vimdiff emerge

Merging:

index.html

Normal merge conflict for \'index.html\':

{local}: modified file

{remote}: modified file

Hit return to start merge resolution tool (opendiff):

Caso você queira usar uma ferramente de merge diferente da padrão (o Git
escolheu opendiff neste caso porque o comando foi executado em um Mac),
você pode ver todas as ferramentas suportadas listadas acima após "one
of the following tools." Você só tem que digitar o nome da ferramenta
que você prefere usar.

  ------------------------------------------------------------------------------------------
  Note   Se você precisa de ferramentas mais avançadas para resolver conflitos mais
         complicados, nós abordamos mais sobre merge em [[Advanced
         Merging]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/r_advanced_merging).
  ------ -----------------------------------------------------------------------------------

  ------------------------------------------------------------------------------------------

Após você sair da ferramenta, o Git pergunta se a operação foi bem
sucedida. Se você responder que sim, o Git adiciona o arquivo ao stage
para marcá-lo como resolvido. Você pode executar git status novamente
para verificar que todos os conflitos foram resolvidos:

\$ git status

On branch master

All conflicts fixed but you are still merging.

(use \"git commit\" to conclude merge)

Changes to be committed:

modified: index.html

Se você estiver satisfeito e verificar que tudo o que havia conflitos
foi testado, você pode digitar git commit para finalizar o commit. A
mensagem de confirmação por padrão é semelhante a esta:

Merge branch \'iss53\'

Conflicts:

index.html

\#

\# It looks like you may be committing a merge.

\# If this is not correct, please remove the file

\# .git/MERGE_HEAD

\# and try again.

\# Please enter the commit message for your changes. Lines starting

\# with \'#\' will be ignored, and an empty message aborts the commit.

\# On branch master

\# All conflicts fixed but you are still merging.

\#

\# Changes to be committed:

\# modified: index.html

\#

Se você acha que seria útil para outras pessoas olhar para este merge no
futuro, você pode modificar esta mensagem de confirmação com detalhes
sobre como você resolveu o conflito e explicar por que você fez as
mudanças que você fez se elas não forem óbvias.

## **8- Gestão de Branches**

Agora que você criou, mesclou e excluiu alguns branches, vamos dar uma
olhada em algumas ferramentas de gerenciamento de branches que serão
úteis quando você começar a usar o tempo todo.

O comando git branch faz mais do que apenas criar e excluir branches. Se
você executá-lo sem argumentos, obterá uma lista simples de seus
branches atuais:

\$ git branch

iss53

\* master

testing

Observe o caractere \* que no início do master: ele indica o branch que
você fez check-out (ou seja, o branch para o qual HEAD aponta). Isso
significa que se você fizer commit neste ponto, o branch master será
movido para frente com seu novo trabalho. Para ver o último commit em
cada branch, você pode executar git branch -v:

\$ git branch -v

iss53 93b412c Fix javascript issue

\* master 7a98805 Merge branch \'iss53\'

testing 782fd34 Add scott to the author list in the readme

As opções \--merged e \--no-merged podem filtrar esta lista para
branches que você tem ou ainda não mesclou no branch em que está
atualmente. Para ver quais branches já estão mesclados no branch em que
você está, você pode executar git branch \--merged:

\$ git branch \--merged

iss53

\* master

Como você já mesclou o iss53 anteriormente, você o vê na sua lista.
Branches que aparecem na lista sem o \* na frente deles geralmente podem
ser deletados com git branch -d; você já incorporou o trabalho deles em
outro branch, então não vai perder nada.

Para ver todos os branches que contêm trabalhos que você ainda não
mesclou, você pode executar git branch \--no-merged:

\$ git branch \--no-merged

testing

Isso mostra seu outro branch. Por conter trabalho que ainda não foi
mesclado, tentar excluí-lo com git branch -d irá não irá executar:

\$ git branch -d testing

error: The branch \'testing\' is not fully merged.

If you are sure you want to delete it, run \'git branch -D testing\'.

Se você realmente deseja excluir o branch e perder esse trabalho, pode
forçá-lo com -D, como mostra a mensagem.

As opções descritas acima, \--merged e \--no-merged irão, se não for
dado um nome de commit ou branch como argumento, mostrar o que é,
respectivamente, mesclado ou não mesclado em seu branch *current*.

Você sempre pode fornecer um argumento adicional para perguntar sobre o
estado de mesclagem em relação a algum outro branch sem verificar esse
outro branch primeiro, como: O que não foi feito merge no branch master?

\$ git checkout testing

\$ git branch \--no-merged master

topicA

featureB

## **9- Fluxo de Branches**

Agora que você tem o básico sobre branches e merges, o que você pode ou
deve fazer com eles? Nesta seção, cobriremos alguns fluxos de trabalho
comuns que o branch torna possível, para que você possa decidir se
deseja incorporá-los em seu próprio ciclo de desenvolvimento.

### **9.1- Branches de longa duração**

Como o Git usa uma mesclagem simples de três vias, mesclar de um branch
para outro várias vezes durante um longo período é geralmente fácil de
fazer. Isso significa que você pode ter vários branches que estão sempre
abertos e que você usa para diferentes estágios do seu ciclo de
desenvolvimento; você pode mesclar regularmente alguns deles com outros.

Muitos desenvolvedores Git têm um fluxo de trabalho que adota essa
abordagem, como ter apenas código totalmente estável em seu branch
master - possivelmente apenas código que foi ou será lançado. Eles têm
outro branch paralelo chamado developers ou next, a partir do qual
trabalham ou usam para testar a estabilidade - não é necessáriamente
sempre estável, mas sempre que chega a um estado estável, pode ser
mesclado com o master. É usado para puxar branches de tópico (de curta
duração, como seu anterior iss53) quando eles estão prontos, para
garantir que eles passem em todos os testes e não introduzam bugs.

Na realidade, estamos falando sobre indicadores que aumentam a linha de
commits que você está fazendo. Os branches estáveis ​​estão mais abaixo na
linha em seu histórico de commit, e os branches mais avançados estão
mais adiante no histórico.

![A linear view of progressive-stability
branching.](media/image9.png){width="6.267716535433071in"
height="0.7916666666666666in"}

Figure 26. Uma visão linear de branches de estabilidade progressiva

Geralmente é mais fácil pensar neles como silos de trabalho, onde
conjuntos de commits passam para um silo mais estável quando são
totalmente testados.

![A \`\`silo\'\' view of progressive-stability
branching.](media/image38.png){width="6.267716535433071in"
height="2.8055555555555554in"}

Figure 27. A visão de "silo" de branches de estabilidade progressiva

Você pode continuar fazendo isso por vários níveis de estabilidade.
Alguns projetos maiores também têm um ramo proposto ou pu (proposed
updates) que tem branches integrados que podem não estar prontos para ir
para o branch next ou master. A ideia é que seus ramos estejam em vários
níveis de estabilidade; quando eles atingem um nível mais estável, eles
são mesclados no ramo acima deles. Novamente, não é necessário ter
vários branches de longa duração, mas geralmente é útil, especialmente
quando você está lidando com projetos muito grandes ou complexos.

### **9.2- Branches por tópicos**

Branches de tópicos, no entanto, são úteis em projetos de qualquer
tamanho. Um branch de tópico é um branch de curta duração que você cria
e usa para um único recurso específico ou trabalho relacionado. Isso é
algo que você provavelmente nunca fez com um VCS antes porque geralmente
é muito difícil para criar e mesclar branches. Mas no Git é comum criar,
trabalhar, mesclar e excluir branches várias vezes ao dia.

Você viu isso na última seção com os branches iss53 e hotfix que você
criou. Você fez alguns commits neles e os deletou diretamente após
fundi-los em seu branch principal. Esta técnica permite que você mude de
contexto de forma rápida e completa - porque seu trabalho é separado em
silos onde todas as mudanças naquele branch têm a ver com aquele tópico,
é mais fácil ver o que aconteceu durante a revisão de código. Você pode
manter as alterações lá por minutos, dias ou meses e mesclá-las quando
estiverem prontas, independentemente da ordem em que foram criadas ou
trabalhadas.

Considere um exemplo de como fazer algum trabalho (no master),
ramificando para um problema (iss91), trabalhando nisso um pouco,
ramificando o segundo branch para tentar outra maneira de lidar com a
mesma coisa (iss91v2), voltando ao seu branch master e trabalhando lá
por um tempo, e então ramificando para fazer algum trabalho que você não
tem certeza se é uma boa ideia (branch dumbidea). Seu histórico de
commits será parecido com isto:

![Multiple topic
branches.](media/image7.png){width="6.267716535433071in"
height="4.972222222222222in"}

Figure 28. Branches de tópico múltiplos

Agora, digamos que você decida que prefere a segunda solução para o seu
problema (iss91v2); e você mostrou o branch dumbidea para seus colegas
de trabalho, e acabou sendo um gênio. Você pode descartar o branch iss91
original (perdendo commits C5 e C6) e mesclar os outros dois. Seu
histórico será assim:

![History after merging \`dumbidea\` and
\`iss91v2\`.](media/image12.png){width="6.267716535433071in"
height="6.208333333333333in"}

Figure 29. Histórico após mesclar dumbidea and iss91v2

Iremos entrar em mais detalhes sobre os vários fluxos de trabalho
possíveis para seu projeto Git em
[[\[ch05-distributed-git\]]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/ch05-distributed-git),
portanto, antes de decidir qual esquema de ramificação seu próximo
projeto usará, certifique-se de ler esse capítulo.

É importante lembrar quando você estiver fazendo tudo isso que esses
branches são completamente locais. Quando você está ramificando e
mesclando, tudo está sendo feito apenas em seu repositório Git - nenhuma
comunicação com o servidor está acontecendo.

## **10- Branches remotos**

Referências remotas são referências (ponteiros) em seus repositórios
remotos, incluindo branches, tags e assim por diante. Você pode obter
uma lista completa de referências remotas explicitamente com git
ls-remote \[remote\] ou git remote show \[remote\] para branches
remotos, bem como mais informações. No entanto, uma forma mais comum é
aproveitar as branches de rastreamento remoto.

Branches de rastreamento remoto são referências ao estado de branches
remotas. São referências locais que você não pode mover; eles são
movidos automaticamente para você sempre que você fizer qualquer
comunicação de rede. Branches de rastreamento remoto agem como
marcadores para lembrá-lo de onde estavam as branches em seus
repositórios remotos da última vez que você se conectou a eles.

Eles assumem a forma (remoto)/(branch). Por exemplo, se você quiser ver
como era o branch master em seu branch remoto origin da última vez que
você se comunicou com ele, você deve verificar o branch origin/master.
Se você estava trabalhando em um problema com um parceiro e ele enviou
um push branch iss53, você pode ter seu próprio branch local iss53; mas
o branch no servidor apontaria para o commit em origin/iss53.

Isso pode ser um pouco confuso, então vamos ver um exemplo. Digamos que
você tenha um servidor Git em sua rede em git.ourcompany.com. Se você
clonar a partir dele, o comando clone do Git automaticamente o nomeia
como origin para você, puxa todos os seus dados, cria um ponteiro para
onde seu branch master está e o nomeia origin/master localmente. O Git
também fornece seu próprio branch master local começando no mesmo lugar
que o branch master de origem, então você tem algo a partir do qual
trabalhar.

+----+-----------------------------------------------------------------+
| No | "origin" não é especial                                         |
| te |                                                                 |
|    | Assim como o nome do branch "master" não tem nenhum significado |
|    | especial no Git, tampouco o "origin". Enquanto "master" é o     |
|    | nome padrão para um branch inicial quando você executa git init |
|    | que é a única razão pela qual é amplamente usado,"origin" é o   |
|    | nome padrão para um repositório remoto quando você executa git  |
|    | clone. Se você executar git clone -o booyah em vez disso, terá  |
|    | booyah/master como seu branch remoto padrão.                    |
+====+=================================================================+
+----+-----------------------------------------------------------------+

![Server and local repositories after
cloning.](media/image11.png){width="6.267716535433071in"
height="4.513888888888889in"}

Figure 30. Repositório local e servidor após o clone

Se você fizer algum trabalho em seu branch master local e, nesse
ínterim, outra pessoa enviar um push para git.ourcompany.com e atualizar
seu branch master, então seus históricos avançam de forma diferente.
Além disso, contanto que você fique fora de contato com o servidor de
origem, o ponteiro origin/master não se move.

![Local and remote work can
diverge.](media/image15.png){width="6.267716535433071in"
height="3.861111111111111in"}

Figure 31. Repositório local e remoto podem divergir

Para sincronizar seu trabalho, você executa um comando git fetch origin.
Este comando procura em qual servidor está a "origin" (neste caso, é
git.nossaempresa.com), busca quaisquer dados que você ainda não tenha, e
atualiza seu banco de dados local, movendo seu ponteiro origin/master
para sua nova posição mais atualizada.

**Não digite sua senha todo o tempo**

Se você estiver usando uma URL HTTPS para fazer push, o servidor Git
solicitará seu nome de usuário e senha para autenticação. Por padrão,
ele solicitará essas informações no terminal para que o servidor possa
dizer se você tem permissão para fazer push.

Se você não quiser digitá-lo toda vez que for fazer o push, você pode
configurar um "credential cache". O mais simples é mantê-lo na memória
por alguns minutos, o que você pode configurar facilmente executando git
config \--global credential.helper cache.

Na próxima vez que um de seus colaboradores buscar no servidor, eles
obterão uma referência de onde a versão do servidor do serverfix está no
branch remoto origin/serverfix:

\$ git fetch origin

remote: Counting objects: 7, done.

remote: Compressing objects: 100% (2/2), done.

remote: Total 3 (delta 0), reused 3 (delta 0)

Unpacking objects: 100% (3/3), done.

From https://github.com/schacon/simplegit

\* \[new branch\] serverfix -\> origin/serverfix

É importante notar que quando você faz uma busca que desativa novos
branches remotos de rastreamento, você não tem automaticamente cópias
locais editáveis deles. Em outras palavras, neste caso, você não tem um
novo branch serverfix - você só tem um ponteiro origin/serverfix que
você não pode modificar.

Para mesclar este trabalho em seu branch atual, você pode executar git
merge origin/serverfix. Se você quiser seu próprio branch serverfix com
o qual possa trabalhar, pode basear-se em seu branch remoto:

\$ git checkout -b serverfix origin/serverfix

Branch serverfix set up to track remote branch serverfix from origin.

Switched to a new branch \'serverfix\'

Isso lhe dá um branch local no qual você pode trabalhar que inicia onde
está o origin/serverfix.

### **11- Rastreando Branches**

Fazer check-out de um branch local a partir de um branch remoto cria
automaticamente o que é chamado de "tracking branch" (e o branch que ele
rastreia é chamado de "branch upstream"). "Tracking branch" são branches
locais que têm um relacionamento direto com um branch remoto. Se você
estiver em um branch de rastreamento e digitar git pull, o Git saberá
automaticamente de qual servidor buscar o branch para fazer o merge.

Quando você clona um repositório, geralmente ele cria automaticamente um
branch master que rastreia origin/master. No entanto, você pode
configurar outros branches de rastreamento se desejar - aqueles que
rastreiam branches em outros branches remotos, ou não rastreiam o branch
master. O caso simples é o exemplo que você acabou de ver, executando
git checkout -b \[branch\] \[remotename\]/\[branch\]. Esta é uma
operação suficiente para que o Git forneça a abreviação \--track:

\$ git checkout \--track origin/serverfix

Branch serverfix set up to track remote branch serverfix from origin.

Switched to a new branch \'serverfix\'

Na verdade, isso é tão comum que existe até um atalho para isso. Se o
nome do branch que você está tentando verificar (a) não existe e (b)
corresponde exatamente a um nome em apenas um repositório remoto, o Git
criará um branch de rastreamento para você:

\$ git checkout serverfix

Branch serverfix set up to track remote branch serverfix from origin.

Switched to a new branch \'serverfix\'

Para configurar um branch local com um nome diferente do branch remoto,
você pode usar facilmente a primeira versão com um nome de branch local
diferente:

\$ git checkout -b sf origin/serverfix

Branch sf set up to track remote branch serverfix from origin.

Switched to a new branch \'sf\'

Agora, seu branch local sf irá puxar automaticamente de
origin/serverfix.

Se você já tem um branch local e deseja configurá-lo para um branch
remoto que acabou de puxar, ou deseja alterar o branch upstream que está
rastreando, você pode usar o método -u ou \--set-upstream-to para git
branch para configurá-lo explicitamente a qualquer momento.

\$ git branch -u origin/serverfix

Branch serverfix set up to track remote branch serverfix from origin.

O Atalho Upstream

Quando você tem um branch de rastreamento configurado, pode referenciar
seu branch upstream com as abreviações \@{upstream} ou \@{u}. Então, se
você está no branch master e está rastreando origin/master, você pode
dizer algo como git merge \@{u} ao invés de git merge origin/master se
desejar.

Se você quiser ver quais branches de rastreamento você configurou, você
pode usar a opção -vv para git branch. Isso listará seus branches locais
com mais informações, incluindo o que cada filial está rastreando e se
sua filial local está à frente, atrás ou ambos.

\$ git branch -vv

iss53 7e424c3 \[origin/iss53: ahead 2\] forgot the brackets

master 1ae2a45 \[origin/master\] deploying index fix

\* serverfix f8674d9 \[teamone/server-fix-good: ahead 3, behind 1\] this
should do it

testing 5ea463a trying something new

Portanto, aqui podemos ver que nosso branch iss53 está rastreando
origin/iss53 e está "ahead" por dois, o que significa que temos dois
commits localmente que não são enviados ao servidor. Também podemos ver
que nosso branch master está rastreando origin/master e está atualizado.
Em seguida, podemos ver que nosso branch serverfix está rastreando o
branch server-fix-good em nosso servidor teamone e está à frente de três
e atrás de um, o que significa que há um commit no servidor que não foi
mesclado ainda e três commits localmente que não foram enviados por
push. Finalmente, podemos ver que nosso branch testing não está
rastreando nenhum branch remoto.

É importante observar que esses números são apenas desde a última vez
que você fez um fetch em cada servidor. Este comando não chega aos
servidores, ele está informando sobre o que armazenou localmente em
cache. Se você quiser ficar totalmente atualizado com os números à
frente e atrás, precisará buscar em todos os seus servidores remotos
antes de executá-lo. Você poderia fazer isso assim:

\$ git fetch \--all; git branch -vv

### **12- Fazendo o Pull**

Embora o comando git fetch baixe todas as alterações no servidor que
você ainda não tem, ele não modificará seu diretório de trabalho. Ele
simplesmente obterá os dados para você e permitirá que você mesmo os
mescle. No entanto, existe um comando chamado git pull que é
essencialmente um git fetch seguido imediatamente por um git merge na
maioria dos casos. Se você tiver um branch de rastreamento configurado
conforme demonstrado na última seção, seja explicitamente configurando-o
ou tendo-o criado para você pelos comandos clone ou checkout, git pull
irá procurar qual servidor e branch seu branch atual está rastreando,
buscará naquele servidor e, em seguida, tentará mesclar nesse branch
remoto.

Geralmente é melhor usar os comandos fetch e merge explicitamente, já
que o git pull muitas vezes pode ser confuso.

### **13- Removendo Branches remotos**

Suponha que você terminou com um branch remoto - digamos que você e seus
colaboradores terminaram com um recurso e o fundiram no branch master do
seu servidor remoto (ou em qualquer branch em que sua linha de código
estável esteja). Você pode deletar um branch remoto usando a opção
\--delete para git push. Se você deseja excluir seu branch serverfix do
servidor, execute o seguinte:

\$ git push origin \--delete serverfix

To https://github.com/schacon/simplegit

\- \[deleted\] serverfix

Basicamente, tudo o que isso faz é remover o ponteiro do servidor. O
servidor Git geralmente mantém os dados lá por um tempo até que seja
removido definitivamente pelo Git, então, se ele foi excluído
acidentalmente, geralmente é fácil de recuperar.

![\`git fetch\` updates your remote
references.](media/image13.png){width="6.267716535433071in"
height="4.597222222222222in"}

Figure 32. git fetch atualiza suas preferências remotas

Para demonstrar a existência de vários servidores remotos e como as
branches remotas desses projetos remotos se parecem, vamos supor que
você tenha outro servidor Git interno usado apenas para desenvolvimento
por uma de suas equipes. Este servidor está em git.team1.ourcompany.com.
Você pode adicioná-lo como uma nova referência remota ao projeto em que
está trabalhando atualmente executando o comando git remote add conforme
abordamos em
[[\[ch02-git-basics\]]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/ch02-git-basics).
Nomeie este servidor remoto como teamone, que será o seu apelido para
toda a URL.

![Adding another server as a
remote.](media/image6.png){width="6.267716535433071in" height="4.5in"}

Figure 33. Adicionando outro servidor como remoto

Agora, você pode executar git fetch teamone para buscar tudo o que o
servidor remoto teamone tem que você ainda não tem. Porque esse servidor
tem um subconjunto dos dados que seu servidor origin tem agora, o Git
não busca nenhum dado, mas define um branch remoto de rastreamento
chamado teamone/master para apontar para o commit que teamone tem como
seu branch master.

![Remote tracking branch for
\`teamone/master\`.](media/image2.png){width="6.267716535433071in"
height="4.486111111111111in"}

Figure 34. Branch remoto de rastreamento para teamone/master

### **14- Empurrando (Push)**

Quando você deseja compartilhar um branch com o mundo, você precisa
transferi-lo para um servidor remoto ao qual você tenha acesso de
gravação. Seus branches locais não são sincronizados automaticamente com
os branches remotos para os quais você escreve - você tem que empurrar
explicitamente os branches que deseja compartilhar. Dessa forma, você
pode usar branches privados para o trabalho que não deseja compartilhar
e fazer o push apenas dos branches de tópicos nos quais deseja
colaborar.

Se você tem um branch chamado serverfix que deseja trabalhar com outros,
pode enviá-lo da mesma forma que fez o push do primeiro branch. Execute
git push \<remote\> \<branch\>:

\$ git push origin serverfix

Counting objects: 24, done.

Delta compression using up to 8 threads.

Compressing objects: 100% (15/15), done.

Writing objects: 100% (24/24), 1.91 KiB \| 0 bytes/s, done.

Total 24 (delta 2), reused 0 (delta 0)

To https://github.com/schacon/simplegit

\* \[new branch\] serverfix -\> serverfix

Este é um atalho. O Git expande automaticamente o branch serverfix para
refs/heads/serverfix:refs/heads/serverfix, o que significa, "Pegue meu
branch local serverfix e empurre-o para atualizar o branch serverfix
remoto". Veremos a parte refs/heads/ em detalhes em
[[\[ch10-git-internals\]]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/ch10-git-internals),
mas geralmente você pode deixá-la desativada. Você também pode fazer git
push origin serverfix:serverfix, que faz a mesma coisa - "Pegue meu
serverfix e torne-o o serverfix remoto." Você pode usar esse formato
para enviar um branch local para um branch remoto com um nome diferente.
Se você não quiser que ele seja chamado de serverfix no remoto, você
pode executar git push origin serverfix:awesomebranch para enviar seu
branch local serverfix para o branch awesomebranch no projeto remoto.

## **15- Rebase**

No Git, existem duas maneiras principais de integrar as mudanças de um
branch para outro: o merge e o rebase. Nesta seção, você aprenderá o que
é o rebase, como fazê-lo, por que é uma ferramenta incrível e em que
casos não vai querer usá-la.

### **15.1- O básico do Rebase**

Se você voltar a um exemplo anterior de [[Mesclagem
Básica]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/r_basic_merging),
você pode ver que o seu trabalho divergiu e fez commits em dois branches
diferentes.

![Simple divergent
history.](media/image21.png){width="6.267716535433071in"
height="2.9027777777777777in"}

Figure 35. Um simples histórico de divergência

A maneira mais fácil de integrar os branches, como já vimos, é o comando
merge. Ele realiza uma fusão de três vias entre os dois últimos
snapshots de branch (C3 e C4) e o ancestral comum mais recente dos dois
(C2), criando um novo snapshot (e commit).

![Merging to integrate diverged work
history.](media/image8.png){width="6.267716535433071in"
height="2.3194444444444446in"}

Figure 36. Fazendo um merge para integrar áreas de trabalho que
divergiram

No entanto, há outra maneira: você pode pegar o patch da mudança que foi
introduzida no C4 e reaplicá-lo em cima do C3. No Git, isso é chamado de
*rebasing*. Com o comando rebase, você pode pegar todas as alterações
que foram confirmadas em um branch e reproduzi-las em outro.

Neste exemplo, você executaria o seguinte:

\$ git checkout experiment

\$ git rebase master

First, rewinding head to replay your work on top of it\...

Applying: added staged command

Ele funciona indo para o ancestral comum dos dois branches (aquele em
que você está e aquele em que você está fazendo o rebase), obtendo o
diff introduzido por cada commit do branch em que você está, salvando
esses diffs em arquivos temporários, redefinindo o branch atual para o
mesmo commit do branch no qual você está fazendo o rebase e, finalmente,
aplicando cada mudança por vez.

![Rebasing the change introduced in \`C4\` onto
\`C3\`.](media/image17.png){width="6.267716535433071in"
height="1.7638888888888888in"}

Figure 37. Fazendo o Rebase da mudança introduzida no C4 em C3

Neste ponto, você pode voltar ao branch master e fazer uma fusão rápida.

\$ git checkout master

\$ git merge experiment

![Fast-forwarding the master
branch.](media/image4.png){width="6.267716535433071in"
height="1.7361111111111112in"}

Figure 38. Fazendo uma fusão rápida no branch master

Agora, o snapshot apontado por C4\' é exatamente o mesmo que foi
apontado por C5 no exemplo de merge. Não há diferença no produto final
da integração, mas o rebase contribui para um histórico mais limpo. Se
você examinar o log de um branch que foi feito rebase, parece uma
registro linear: parece que todo o trabalho aconteceu em série, mesmo
quando originalmente aconteceu em paralelo.

Frequentemente, você fará isso para garantir que seus commits sejam
aplicados de forma limpa em um branch remoto - talvez em um projeto para
o qual você está tentando contribuir, mas que não mantém. Neste caso,
você faria seu trabalho em um branch e então realocaria seu trabalho em
origin/master quando estivesse pronto para enviar seus patches para o
projeto principal. Dessa forma, o mantenedor não precisa fazer nenhum
trabalho de integração - apenas um fusão rápida ou uma aplicação limpa.

Observe que o snapshot apontado pelo commit final com o qual você
termina, seja o último dos commits para um rebase ou o commit final de
mesclagem após um merge, é o mesmo snapshot - é apenas o histórico que é
diferente. O Rebase reproduz as alterações de uma linha de trabalho para
outra na ordem em que foram introduzidas, enquanto a mesclagem pega os
finais e os mescla.

### **16- Rebases mais interessantes**

Você também pode fazer o replay do rebase em algo diferente do branch de
destino. Pegue um histórico como [[Um histórico com um tópico de branch
de outro
branch]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/rrbdiag_e),
por exemplo. Você ramificou um branch de tópico (server) para adicionar
alguma funcionalidade do lado do servidor ao seu projeto e fez um
commit. Então, você ramificou isso para fazer as alterações do lado do
cliente (client) e fez commit algumas vezes. Finalmente, você voltou ao
seu branch de servidor e fez mais alguns commits.

![A history with a topic branch off another topic
branch.](media/image3.png){width="6.267716535433071in"
height="3.5416666666666665in"}

Figure 39. Um histórico com um tópico de branch de outro branch

Suponha que você decida que deseja mesclar suas alterações do lado do
cliente em sua linha principal para um lançamento, mas deseja adiar as
alterações do lado do servidor até que seja testado mais profundamente.
Você pode pegar as mudanças no cliente que não estão no servidor (C8 e
C9) e reproduzi-las em seu branch master usando a opção \--onto do git
rebase:

\$ git rebase \--onto master server client

Isso basicamente diz: "Pegue o branch client, descubra os patches, desde
que divergiu do branch server, e repita esses patches no branch client
como se fosse baseado diretamente no branch master". É um pouco
complexo, mas o resultado é bem legal.

![Rebasing a topic branch off another topic
branch.](media/image19.png){width="6.267716535433071in"
height="2.4583333333333335in"}

Figure 40. Rebase o tópico de um branch de outro branch

Agora, você pode fazer uma fusão rápida no branch master (veja [[Avanço
rápido de seu branch principal para incluir as alterações da branch do
cliente]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/rrbdiag_g)):

\$ git checkout master

\$ git merge client

![Fast-forwarding your master branch to include the client branch
changes.](media/image16.png){width="6.267716535433071in"
height="1.9722222222222223in"}

Figure 41. Avanço rápido de seu branch principal para incluir as
alterações da branch do cliente

Digamos que você decida puxar seu branch de servidor também. Você pode
realocar o branch do servidor no branch master sem ter que verificá-lo
primeiro executando git rebase \[basebranch\] \[topicbranch\] - que
verifica o branch do tópico (neste caso, server) para você e repete no
branch base (master):

\$ git rebase master server

Isso reproduz o trabalho do server em cima do trabalho do master, como
mostrado em [[Rebase o branch server por cima do branch
master]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/rrbdiag_h).

![Rebasing your server branch on top of your master
branch.](media/image35.png){width="6.267716535433071in"
height="1.0138888888888888in"}

Figure 42. Rebase o branch server por cima do branch master

Então, você pode avançar o branch base (master):

\$ git checkout master

\$ git merge server

Você pode remover os branches client e server porque todo o trabalho foi
integrado e você não precisa mais deles, deixando seu histórico para
todo o processo parecido com [[Histórico final de
commits]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/rrbdiag_i):

\$ git branch -d client

\$ git branch -d server

![Final commit history.](media/image34.png){width="6.267716535433071in"
height="0.6388888888888888in"}

Figure 43. Histórico final de commits

### **17- Os perigos do Rebase**

Ahh, mas a felicidade do rebase não vem sem suas desvantagens, que podem
ser resumidas em uma única linha:

**Não faça rebase de commits que existam fora do seu repositório.**

Se você seguir essa diretriz, ficará bem. Do contrário, as pessoas irão
odiá-lo e você será desprezado por amigos e familiares.

Quando você faz o rebase, você está abandonando commits existentes e
criando novos que são semelhantes, mas diferentes. Se você enviar
commits para algum lugar e outras pessoas fizerem o pull e basearem seu
trabalho neles, e então você reescrever esses commits com git rebase e
enviá-los novamente, seus colaboradores terão que fazer um novo merge em
seus trabalhos e as coisas ficarão complicadas quando você tentar puxar
o trabalho deles de volta para o seu.

Vejamos um exemplo de como o rebase de um trabalho que você tornou
público pode causar problemas. Suponha que você clone de um servidor
central e faça algum trabalho a partir dele. Seu histórico de commits é
parecido com este:

![Clone a repository, and base some work on
it.](media/image22.png){width="6.267716535433071in"
height="3.7083333333333335in"}

Figure 44. Fazendo o clone de um repositório e trabalhando com ele

Agora, outra pessoa faz mais alterações que inclui um merge e envia esse
trabalho para o servidor central. Você o busca e mescla o novo branch
remoto em seu trabalho, fazendo com que seu histórico se pareça com
isto:

![Fetch more commits, and merge them into your
work.](media/image10.png){width="6.267716535433071in"
height="3.9722222222222223in"}

Figure 45. Buscar mais commits e fazer merge em seu trabalho

Em seguida, a pessoa que enviou o trabalho mesclado decide voltar atrás
e realizar um rebase no trabalho em vez disso; ele faz um git push
\--force para sobrescrever o histórico no servidor. Você então busca
daquele servidor, derrubando os novos commits.

![Someone pushes rebased commits, abandoning commits you've based your
work on.](media/image5.png){width="6.267716535433071in"
height="3.9722222222222223in"}

Figure 46. Alguém empurra commits que foram feitos rebase, abandonando
commits nos quais você baseou seu trabalho

Agora você está em apuros. Se você fizer um git pull, você criará um
commit de merge que inclui as duas linhas do histórico, e seu
repositório ficará assim:

![You merge in the same work again into a new merge
commit.](media/image14.png){width="6.267716535433071in"
height="3.2916666666666665in"}

Figure 47. Você faz o merge do mesmo trabalho novamente em um novo
commit de merge

Se você executar um git log quando seu histórico estiver assim, você
verá dois commits com o mesmo autor, data e mensagem, o que será
confuso. Além disso, se você enviar esse histórico de volta ao servidor,
reintroduzirá todos os commits realocados no servidor central, o que
pode confundir ainda mais as pessoas. É bastante seguro assumir que o
outro desenvolvedor não quer que C4 e C6 apareçam na história; é por
isso que eles fizeram um rebase antes.

### **18- Rebase quando vocês faz Rebase**

Se você **realmente** se encontrar em uma situação como essa, o Git tem
mais alguma mágica que pode te ajudar. Se alguém em sua equipe forçar
mudanças que substituam o trabalho no qual você se baseou, seu desafio é
descobrir o que é seu e o que eles reescreveram.

Acontece que, além da soma de verificação SHA-1 de commit, o Git também
calcula uma soma de verificação que é baseada apenas no patch
introduzido com a confirmação. Isso é chamado de "patch-id".

Se você puxar o trabalho que foi reescrito e fazer o rebase sobre os
novos commits de seu parceiro, o Git pode muitas vezes descobrir o que é
exclusivamente seu e aplicá-lo de volta ao novo branch.

Por exemplo, no cenário anterior, se em vez de fazer uma fusão quando
estamos em [[Alguém empurra commits que foram feitos rebase, abandonando
commits nos quais você baseou seu
trabalho]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/r_pre_merge_rebase_work)
executarmos git rebase teamone/master, Git irá:

- Determinar qual trabalho é exclusivo para nosso branch (C2, C3, C4,
  C6, C7)

- Determinar quais não são confirmações de merge (C2, C3, C4)

- Determinar quais não foram reescritos no branch de destino (apenas C2
  e C3, uma vez que C4 é o mesmo patch que C4\')

- Aplicar esses commits no topo de teamone/master

Então, em vez do resultado que vemos em [[Você faz o merge do mesmo
trabalho novamente em um novo commit de
merge]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/r_merge_rebase_work),
acabaríamos com algo mais parecido com [[Rebase on top of force-pushed
rebase
work.]{.underline}](https://git-scm.com/book/pt-br/v2/ch00/r_rebase_rebase_work).

![Rebase on top of force-pushed rebase
work.](media/image1.png){width="6.267716535433071in"
height="3.2916666666666665in"}

Figure 48. Rebase on top of force-pushed rebase work.

Isso só funciona se C4 e C4\' que seu parceiro fez forem quase
exatamente o mesmo patch. Caso contrário, o rebase não será capaz de
dizer o que é uma duplicata e adicionará outro patch semelhante ao C4
(que provavelmente não será aplicado, uma vez que as alterações já
estariam pelo menos um pouco lá).

Você também pode simplificar isso executando um git pull \--rebase em
vez de um git pull normal. Ou você poderia fazer isso manualmente com um
git fetch seguido por um git rebase teamone/master neste caso.

Se você estiver usando git pull e quiser tornar \--rebase o padrão, você
pode definir o valor de configuração pull.rebase com algo como git
config \--global pull.rebase true.

Se você tratar o rebase como uma forma de limpar e trabalhar com commits
antes de enviá-los, e se você apenas fazer o rebase dos commits que
nunca estiveram disponíveis publicamente, então você ficará bem. Se você
fizer o rebase dos commits que já foram enviados publicamente, e as
pessoas podem ter baseado o trabalho nesses commits, então você pode ter
alguns problemas frustrantes e o desprezo de seus companheiros de
equipe.

Se você ou um parceiro achar necessário em algum ponto, certifique-se de
que todos saibam executar git pull \--rebase para tentar tornar a dor
depois que ela acontecer um pouco mais simples.

### **19- Rebase vs. Merge**

Agora que você viu o rebase e o merge em ação, pode estar se perguntando
qual é o melhor. Antes de respondermos, vamos voltar um pouco e falar
sobre o que a história significa.

Um ponto de vista sobre isso é que o histórico de commit do seu
repositório é um **registro do que realmente aconteceu.** É um documento
histórico, valioso por si só, e não deve ser alterado. Desse ângulo,
mudar o histórico de commits é quase uma blasfêmia; você está mentindo
sobre o que realmente aconteceu. E daí se houvesse uma série confusa de
commits de merge? Foi assim que aconteceu, e o repositório deve
preservar isso para a posteridade.

O ponto de vista oposto é que o histórico de commits é a **história de
como seu projeto foi feito.** Você não publicaria o primeiro rascunho de
um livro, e o manual de como manter seu software merece uma edição
cuidadosa. Este é o campo que usa ferramentas como rebase e
filter-branch para contar a história da maneira que for melhor para
futuros leitores.

Agora, à questão de saber se merge ou rebase é melhor: espero que você
veja que não é tão simples. O Git é uma ferramenta poderosa e permite
que você faça muitas coisas para e com sua história, mas cada equipe e
cada projeto são diferentes. Agora que você sabe como essas duas coisas
funcionam, cabe a você decidir qual é a melhor para sua situação
específica.

Em geral, a maneira de obter o melhor dos dois mundos é fazer o rebase
nas mudanças locais que você fez, mas não compartilhou ainda antes de
empurrá-las para limpar seu histórico, mas nunca faça rebase em algo que
você empurrou em algum lugar.
