# Git & Github

Se você está tentando desenvolver uma comunidade de código aberto ou colaborar em seus próprios projetos, é essencial saber como bifurcar e gerar pedidos de `pull request` corretamente. Infelizmente, é muito fácil cometer erros ou não saber o que você deve fazer quando estiver aprendendo o processo inicialmente. Eu sei que certamente tive um problema inicial considerável como ele, e achei que muitas das informações no Github e na internet eram bastante fragmentadas e incompletas e decidimos criar este novo tutorial.

Em uma tentativa de coletar essas informações para mim e para os outros membros de nossos projetos, este breve tutorial é o que descobrimos juntos como procedimento bastante padronizado para criar um fork, fazer seu trabalho localmente, emitir uma solicitação de `pull request` e `merge` das solicitações através do pull request ao projeto original.

## Criando seu primeiro fork

Basta ir até a página do Github do projeto a qual deseja contribuir e clicar no botão `fork`. É simples assim. Uma vez feito isso, você pode sar seu cliente git favorito para clonar seu repositório

Certo, você realizou o seu primeiro fork agora vamos baixar os dados para sua máquina usando a linha de comando:

`` `shell
# Clone seu fork para sua máquina local
git clone git@github.com:{username}/{forked-projet.git}
`` `

**GIT IDE**
Saiba um pouco mais sobre IDE's git e como elas podem te ajudar no seu dia-a-dia

[Github Desktop](https://desktop.github.com/) | Gratuito
[GitKraken](https://www.gitkraken.com/) | Gratuito para projetos opensource

## Mantendo seu Fork atualizado

Embora este não seja um passo absolutamente necessário, se você planeja fazer algo mais do que apenas uma pequena correção rápida no projeto, você vai querer ter certeza de manter seu fork atualizado acompanhando as atualização do repositório "upstream" original que você bifurcou.
Para fazer isto, você precisá adicionar um controle remoto:

`` `
 # Adicionando um repositório 'upstream' à seus remote
 git remote add upstream https://github.com/{upstream-user}/{original-projet}.git

 # Verificando se foi adicionado ao seu remote
 git remote -v
`` `

Sempre que você quiser atualizar seu `fork` com as alterações mais recentes do upstream, você precisará primeiro buscar as ramificações do repositório upstream e os commits mais recentes para trazê-los para o seu repositório:
`` `shell
# Buscar atualizações da upstream
git fetch upstream

# Veja todas branchs, incluindo as do upstream
git branch -va
`` `

Agora, finalize sua própria ramificação master e mescle o branch master do repo upstream:
`` `shell
# Checkout seu branch master e mesclar upstream
git checkout master
git merge upstream/master
`` `

Se não houver commits exclusivos no branch master local, o git irá simplesmente executar um avanço rápido. No entanto, se você estiver fazendo alterações no master (na grande maioria dos casos, você provalvelmente não deveria está - [veja a próxima seção](#), você pode ter que lidar com conflitos. tenha cuidado para respeitar as alterações feitas pela `upstream`.
