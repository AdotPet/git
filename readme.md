# ![Github](https://suse.me/sites/default/files/styles/thumbnails_32x32/public/icons/9331c338-165f-43f5-acd3-10ce5afd14c9_11457.png?itok=lWoD9e8c) Git & Github 
[![contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)]()
![](https://img.shields.io/github/license/adotpet/adotpet.svg)
![](https://img.shields.io/github/last-commit/adotpet/git.svg)
![](https://img.shields.io/github/commit-status/adotpet/git/master/d634f9aab0fe1286958c0e5ebd66f03e89da892a.svg)

Se você está tentando desenvolver uma comunidade de código aberto ou colaborar em seus próprios projetos, é essencial saber como bifurcar e gerar pedidos de `pull request` corretamente. Infelizmente, é muito fácil cometer erros ou não saber o que você deve fazer quando estiver aprendendo o processo inicialmente. Eu sei que certamente tive um problema inicial considerável como ele, e achei que muitas das informações no Github e na internet eram bastante fragmentadas e incompletas e decidimos criar este novo tutorial.

Em uma tentativa de coletar essas informações para mim e para os outros membros de nossos projetos, este breve tutorial é o que descobrimos juntos como procedimento bastante padronizado para criar um fork, fazer seu trabalho localmente, emitir uma solicitação de `pull request` e `merge` das solicitações através do pull request ao projeto original.

## Criando seu primeiro fork

Basta ir até a página do Github do projeto a qual deseja contribuir e clicar no botão `fork`. É simples assim. Uma vez feito isso, você pode sar seu cliente git favorito para clonar seu repositório

Certo, você realizou o seu primeiro fork agora vamos baixar os dados para sua máquina usando a linha de comando:

```shell
# Clone seu fork para sua máquina local
git clone git@github.com:{username}/{forked-projet.git}
```

**GIT IDE**
Saiba um pouco mais sobre IDE's git e como elas podem te ajudar no seu dia-a-dia

- [Github Desktop](https://desktop.github.com/) | Gratuito
- [GitKraken](https://www.gitkraken.com/) | Gratuito para projetos opensource

## Mantendo seu Fork atualizado

Embora este não seja um passo absolutamente necessário, se você planeja fazer algo mais do que apenas uma pequena correção rápida no projeto, você vai querer ter certeza de manter seu fork atualizado acompanhando as atualização do repositório "upstream" original que você bifurcou.
Para fazer isto, você precisá adicionar um controle remoto:

```
 # Adicionando um repositório 'upstream' à seus remote
 git remote add upstream https://github.com/{upstream-user}/{original-projet}.git

 # Verificando se foi adicionado ao seu remote
 git remote -v
```

Sempre que você quiser atualizar seu `fork` com as alterações mais recentes do upstream, você precisará primeiro buscar as ramificações do repositório upstream e os commits mais recentes para trazê-los para o seu repositório:

```shell
# Buscar atualizações da upstream
git fetch upstream

# Veja todas branchs, incluindo as do upstream
git branch -va
```

Agora, finalize sua própria ramificação master e mescle o branch master do repo upstream:

```shell
# Checkout seu branch master e mesclar upstream
git checkout master
git merge upstream/master
```

Se não houver commits exclusivos no branch master local, o git irá simplesmente executar um avanço rápido. No entanto, se você estiver fazendo alterações no master (na grande maioria dos casos, você provalvelmente não deveria está - [veja a próxima seção](#), você pode ter que lidar com conflitos. tenha cuidado para respeitar as alterações feitas pela `upstream`.

agora, sua branch master local está atualizada com tudo que foi modificado.

## Contribuindo com seu trabalho

### Criando uma Branch

Sempre que você começa a trabalhar em um novo recurso ou bugfix, é importante criar uma nova `branch`. Não é apenas o fluxo de trabalho adequado do git, mas também mantém suas alterações organizadas e separadas do branch master para que você possa enviar e gerenciar facilmente várias solicitações de `pull request` para cada tarefa que concluir.

Como criar uma nova ramificação e começar a trabalhar nela:

```
# Checkout o branch master - você quer que seu novo branch tenha todas as informações do master
git checkout master

# Crie uma nova branch chamada feature/header
git branch feature/header

# Mudando para a sua nova branch de trabalho
git checkout feature/header
```

Agora, você pode iniciar as modificações em sua nova branch de trabalho.

Se você usa o conceito (Master recebe Develop recebe Features)
ao terminar seu trabalho você deverá fazer o seu merge a develop local e ao finalizar todas as moficações que sempre são enviadas ao seu develop local, você irá digitar:

```
# Movendo modificações da develop local para a master local
# Dentro da sua branch master
git merge develop
```

## Enviando um Pull Resquest

### Organizando seu projeto

Antes de enviar sua primeira solicitação de `pull request`, você pode querer fazer algumas coisas para organizar sua branch e torná-la o mais simples possível para o `upstream` master possa testar, aceita e realizar o `merge` do seu trabalho.

Se algum commit tiver sido feito para a branch master do seu upstream, você deve efetuar um rebase na sua área branch de desenvolvimento para que o `merge` seja um `fast-forward` para não exigir um trabalho com conflitos.

```
# Fetch upstream master e merge com seu repositório master
git fetch upstream
git checkout master
git merge upstream/master

# Se houver novos commits, efetue um rebase do seu branch de desenvolvimento
git checkout {branch-name}
git rebase master
```

Agora, pode ser desejável eliminar alguns dos seus commits menores em um pequeno número de commits maiores e mais coesos. você pode fazer isso com um `rebase` interativo

```
# Rebase todos os commits no seu branch de desenvolvimento
git checkout
git rebase -i master
```

Isso irá abrir um editor de texto onde você pode especificar quais commits para `squash`

## Enviando seu Pull Request

Depois de confirmar e enviar todas as alterações para o GitHub, vá até a página do seu `Fork` no GitHub, selecione sua `branch` de desenvolvimento e clique no botão de solicitação de `pull request`. Se você precisar fazer algum ajuste na solicitação de pull, basta enviar as atualizações para o GitHub. Sua solicitação de `pull request` rastreará automaticamente as alterações em sua `branch` de desenvolvimento e atualização.

## Aceitando e mesclando uma solicitação pull request

Observe que, diferentemente das seções anteriores que foram escritas a partir da perspectiva de alguém que criou uma `branch` e gerou uma solicitação `pull request`, essa seção é gravada da perspectiva do proprietário do repositório original que está manipulando uma solicitação de recebindo de código. Assim, onde o "forker" estava se referindo ao repositório original como `upstream`, agora estamos olhando para ele como o dono daquele repositório original e do `remote` padrão.

### Fazer um checkout e testar solicitações de pull request

Abra o arquivo `.git/config` e adicione uma nova linha em `[remote "origin"]`:

```
fetch = +refs/pull/*/head:refs/pull/origin/*
```

Agora você pode buscar e fazer checkout em qualquer solicitação de `pull request` e testar o código antes de realizar o `merge` entre as `branch`.

```
# Fetch todas as branchs com pull request
git fetch origin

# Faça o checkout em uma determinada branch de pull request com base no seu número de pull request
git checkout -b 999 pull/origin/999
```

Lembre-se de que essas ramificações serão somente de leitura e você não poderá enviar nenhuma alteração.

### Merge um Pull request automáticamente

Nos casos em que o `merge` é um fast-foward, você poderá fazer a mesclagem automaticamente apenas clicando no botão na página de solicitações de `pull request` no GitHub.

### Realizando merge manual de uma solicitação pull request

Para fazer a mesclagem manualmente, você precisará fazer o checkout na `branch` de destino no contribuinte e mesclar com sua branch de desenvolvimento e enviar por push.

```
# Finalize o branch que você está mesclando no repositório de destino
git checkout master

# Puxe a branch de desenvolvimento do seu contribuidor, onde o contribuição dele foi realizada
git pull https://github.com/{forkuser}/{forkedrepo}.git release

# Mesclando com seu branch de desenvolvimento
git merge newfeature

# Enviando para seu master com o novo recurso mesclado nele
git push origin master
```

Agora que você terminou com a `branch` de desenvolvimento, você está livre para excluí-la.

```
git branch -d newfeature
```