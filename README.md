git config: Este é o primeiro comando a ser executado após instalar o Git. Ele possibilita configurar seu nome e endereço de email que ficará vinculado às alterações.
$ git config --global user.name "Nome do utilizador"
$ git config --global user.email "seu@email.com"

$ git config --list                                    // Mostra todas as definições
$ git config user.name                                 // Mostra apenas o nome de utilizador
$ git config user.email                                // Mostra apenas o email do utilizador

$ git config --global init.defaultBranch main          // Define o nome padrão de novos repositórios


git init: Inicia ou cria um repositório.
$ dir                                                    // Mostra o diretório atual
$ mkdir nome_da_pasta                                    // Cria uma pasta nova no diretório atual 
$ cd nome_da_pasta                                       // Navega para a pasta
$ git init                                               // Transforma a pasta num repositório, contendo um diretório .git (evitar mexer neste ficheiros)

git status: Permite visualizar o estado do repositório.
$ git status


Ficheiros num repositório podem estar nos seguintes estados:

   Untracked  →  Tracked
       ↓           ↓
   [Ignored]   Unmodified → Modified → Staged
                    ↑                    ↓
                    ←←←←←←←←←←←←←←←←←←←←←
                         (after commit)


git add: Prepara o conteúdo para o próximo commit.
$ git add nome_do_arquivo
$ git add .

git commit: Salva o conteúdo atual junto com uma mensagem de registo do utilizador que descreve as alterações.
$ git commit -m "Mensagem descritiva do commit"

git clone: Clona um repositório existente.
$ git clone [url]

git branch: Uma branch nada mais é do que uma ramificação dentro do repositório. Este comando pode ser utilizado de diversas maneiras.
$ git branch // Mostra as branches existentes em um repositório
$ git branch nome_da_branch // Cria uma nova branch
$ git branch -M nome_da_branch // Renomeia a branch atual

git log: Exibe um histórico de commits. Este comando pode ser utilizado de diversas formas.
$ git log
$ git log --oneline

git remote: Exibe o repositório remoto.
$ git remote
$ git remote -v

git remote add origin urlDoRepositorio: adiciona e indica a URL do repositório remoto em que os arquivos serão mantidos

git push origin master: envia os arquivos para o repositório remoto que você especificou através da URL do comando acima

git checkout -b <nomeDaBranch>: cria uma nova branch

git checkout <nomeDaBranch>: alterna para a branch especificada
