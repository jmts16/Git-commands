git config: Este é o primeiro comando a ser executado após instalar o Git. Ele possibilita configurar seu nome e endereço de email que ficará vinculado às alterações.
   $ git config --global user.name "Nome do utilizador"
   $ git config --global user.email "seu@email.com"

   $ git config --list                                    // Mostra todas as definições
   $ git config user.name                                 // Mostra apenas o nome de utilizador
   $ git config user.email                                // Mostra apenas o email do utilizador

   $ git config --global init.defaultBranch main          // Define o nome padrão de novos repositórios

========================================================================================================================================================================================
git init: Inicia ou cria um repositório.
   $ dir                                                    // Mostra o diretório atual
   $ mkdir nome_da_pasta                                    // Cria uma pasta nova no diretório atual 
   $ cd nome_da_pasta                                       // Navega para a pasta
   $ git init                                               // Transforma a pasta num repositório, contendo um diretório .git (evitar mexer nestes ficheiros)

git status: Permite visualizar o estado do repositório.
   $ git status

Ficheiros num repositório podem estar nos seguintes estados:

   Untracked  →  Tracked
       ↓           ↓
   [Ignored]   Unmodified → Modified → Staged
                    ↑                    ↓
                    ←←←←←←←←←←←←←←←←←←←←←
                         (after commit)

========================================================================================================================================================================================
git add: Prepara o conteúdo para o próximo commit.
   $ git add nome_do_arquivo
   $ git add .

git commit: Salva o conteúdo atual junto com uma mensagem de registo do utilizador que descreve as alterações.
   $ git commit -m "Mensagem descritiva do commit"


Fluxo de trabalho em Git:
      1. Criar/Editar ficheiros
             ↓
      2. git add <ficheiros>      (estádio muda)
             ↓
      3. git commit -m "mensagem" (guarda snapshot)
             ↓
      4. Repetir

Introduzir git init reinicializa o processo

========================================================================================================================================================================================
O fluxo de trabalho em Git funciona da seguinte forma:

┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  Working        │     │    Staging      │     │   Repository    │
│  Directory      │ ──→ │    Area         │ ──→ │   (.git)        │
│                 │     │    (Index)      │     │                 │
│  (Your files)   │     │  (Next commit)  │     │  (All commits)  │
└─────────────────┘     └─────────────────┘     └─────────────────┘
        │                       │                       │
   git add ──────────────────→  │                       │
        │                git commit ──────────────────→ │
        │                       │                       │
   ←──────────────────── git checkout ←─────────────────│

Working Directory:	Ficheiros que se vê e edita
Staging Area:	Mudanças em espera para o próximo commit
Repository:  Armazenamento permanente de todos os commits

========================================================================================================================================================================================
git add tem várias funcionalidades:

# Adicionar um ficheiro específico
   $ git add nomeficheiro.js

# Adicionar vários ficheiros específicos
   $ git add ficheiro1.js ficheiro2.js ficheiro3.js

# Adicionar todos os ficheiros num diretório
   $ git add src/

# Adicionar todas as mudanças no diretório presente
   $ git add .

# Adicionar TODAS as mudanças em todo o lado
   $ git add -A

# Escolher o que adicionar de forma interativa
   $ git add -p

Portanto o git add:
1- Tira um snapshot do conteúdo corrente do ficheiro 
2- Armazena este na staging area
3- Prepares o ficheiro para o próximo commit

========================================================================================================================================================================================
git commit tem várias funcionalidades:

# Commit com mensagem em linha
   $ git commit -m "A sua mensagem"

# Commit com mensagem de várias linhas (abre editor de texto)
   $ git commit

# Adicionar todos os ficheiros tracked e commit
   $ git commit -a -m "Messagem"
      # ou
   $ git commit -am "Messagem"

# Alterar o último commit
   $ git commit --amend

Portanto o git commit:
1- Pega em tudo do staging area
2- Cria um novo objeto comit, com:
     - Um hash SHA único
     - A informação do autor
     - Um timestamp
     - A mensagem de commit
     - Um indicador ao commit pai
3- Atualiza o indicador do branch para o novo commit

========================================================================================================================================================================================
git diff permite ver o que foi alterado no diretório de trabalho (alterações não preparadas):

   $ git diff

# Ver o que está preparado para commit:
   $ git diff --staged
      # ou
   $ git diff --cached

# Comparar dois commits (através de identificadores):
   $ git diff abc123 def456

Exemplo de output diff:

      diff --git a/utils.js b/utils.js
      index 1234567..abcdefg 100644
      --- a/utils.js
      +++ b/utils.js
      @@ -1 +1,5 @@
      function greet() { return 'Hello!'; }
      +
      +function farewell() {
      +  return 'Goodbye!';
      +}

- As linhas que começam com - foram removidas
- As linhas que começam com + foram adicionadas

========================================================================================================================================================================================
VISUALIZAR HISTÓRICO

git log permite visualizar o histórico de commits:

# Histórico completo
   $ git log

# Uma linha por commit
   $ git log --oneline

# Com um gráfico que mostra as branches
   $ git log --oneline --graph

# Últimos 5 commits
   $ git log -5

# Commits de um autor específico
   $ git log --author="O Teu Nome"

# Commits que afetam um ficheiro específico
   $ git log -- nomeficheiro.js

##################################################

git show permite ver um commit específico:

# Mostrar o commit mais recente
   $ git show

# Mostrar um commit específico
   $ git show abc1234

# Mostrar apenas os ficheiros alterados
   $ git show --stat abc1234

========================================================================================================================================================================================
REMOVER E MOVER FICHEIROS

# Remover do Git e do sistema (!)
   $ git rm nomeficheiro.js

# Remover apenas do Git (manter o ficheiro no sistema)
   $ git rm --cached nomeficheiro.js

# Após a remoção, submeter a alteração
   $ git commit -m "Remover nomeficheiro.js"


# Renomear um ficheiro
   $ git mv oldname.js newname.js

      # Isto é equivalente a:
      # mv oldname.js newname.js
      # git rm oldname.js
      # git add newname.js

# Submeter a renomeação
   $ git commit -m "Renomear oldname.js para newname.js"

========================================================================================================================================================================================
BOAS PRÁTICAS

Fazer commits com frequência:
   * Commits pequenos e focados são mais fáceis de compreender
   * É mais fácil encontrar e corrigir erros (*bugs*)
   * É mais fácil rever e reverter alterações

Escreva boas mensagens de commit:
   * Utilize o presente: "Adicionar funcionalidade", em vez de "Adicionada funcionalidade"
   * Seja específico: "Corrigir erro na validação de início de sessão", em vez do genérico "Corrigir erro"
   * Mantenha a primeira linha com menos de 50 caracteres

Fazer stage (add) de forma seletiva:
   * Agrupe apenas alterações relacionadas
   * Utilize `git add -p` para um controlo mais detalhado
   * Reveja as alterações preparadas antes de efetuar o *commit*


========================================================================================================================================================================================


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

