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

- Introduzir git init reinicializa o processo

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
   
################################################################
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
ENTENDER A STAGING AREA

Working Directory          Staging Area           Repository
┌─────────────────┐       ┌─────────────────┐    ┌─────────────────┐
│                 │       │                 │    │                 │
│   file1.js ─────────────→  file1.js ────────→  │    Commit 1     │
│   file2.js      │       │                 │    │    Commit 2     │
│   file3.js ─────────────→  file3.js       │    │    Commit 3     │
│                 │       │                 │    │                 │
└─────────────────┘       └─────────────────┘    └─────────────────┘
                               ↑
                    "Shopping cart" for
                    your next commit


>PORQUÊ UMA STAGING AREA

A área de preparação oferece um controlo detalhado:

Submissões seletivas ->	Escolhe exatamente que alterações submeter
Revisão de alterações ->	Vê o que está prestes a ser submetido
Submissões lógicas -> 	Agrupa alterações relacionadas
Divisão de trabalho ->	Separa alterações não relacionadas em submissões diferentes


>ADICIONAR FICHEIROS
   
   # Adicionar um ficheiro específico
   $ git add filename.js
   
   # Adicionar vários ficheiros
   $ git add file1.js file2.js
   
   # Adicionar todos os ficheiros num diretório
   $ git add src/
   
   # Adicionar todas as alterações (novos, modificados, eliminados)
   $ git add .
   $ git add -A
   
   # Adicionar apenas ficheiros modificados e eliminados (não os novos)
   $ git add -u

>VERIFICAR O QUE ESTÁ NA "STAGING AREA"

   # Ver o estado
   $ git status
   
   # Ver as alterações preparadas (staged)
   $ git diff --staged
   $ git diff --cached  # O mesmo

>REMOVER FICHEIROS DA "STAGING AREA"
   
   # Remover um ficheiro específico da "staging area" (mantendo as alterações)
   $ git restore --staged filename.js
   
   # Sintaxe mais antiga (ainda funciona)
   $ git reset HEAD filename.js
   
   # Remover todos os ficheiros da "staging area"
   $ git restore --staged .
   $ git reset HEAD


>STAGING PARCIAL COM `git add -p`

A opção `-p` (ou `--patch`) permite escolher, de forma interativa, que partes de um ficheiro pretende preparar:

   $ git add -p filename.js

O Git apresenta cada alteração (designada por "hunk" ou bloco) e pergunta o que pretende fazer:

   @@ -1,5 +1,7 @@
   function login() {
   +  // Validação adicionada
   validateInput();
   +  logAttempt(); 
   return authenticate(); 
   }
   Preparar este bloco [y,n,q,a,d,s,e,?]?

Opções:

   y -> (Yes)       Preparar este bloco
   n -> (No)        Não preparar este bloco
   q -> (Quit)      Sair (e não preparar os restantes)
   a -> (All)       Preparar este bloco e todos os restantes
   d -> (Drop)      Não preparar este bloco nem os restantes
   s -> (Separate)  Dividir em blocos mais pequenos
   e -> (Edit)      Editar o bloco manualmente

>FICHEIRO DE ÍNDICE

A área de preparação (*staging area*) é armazenada em `.git/index`. 

Este ficheiro binário regista:
   * Que ficheiros estão na staging area;
   * O seu conteúdo (sob a forma de referências a *blobs*);
   * As permissões dos ficheiros e timestamps;

Pode inspecioná-lo com:

   $ git ls-files --stage


>PREPARAR REMOÇÃO DE FICHEIROS

Quando elimina um ficheiro, precisa de preparar essa remoção:

   # Eliminar e preparar num único comando
   $ git rm filename.js

   # Ou manualmente
   $ rm filename.js
   $ git add filename.js  # Prepara a remoção


>PREPARAR MUDANÇA DE NOME DE FICHEIROS

O Git deteta mudanças de nome através da semelhança de conteúdo:

   # Mudar o nome e preparar num único comando
   $ git mv oldname.js newname.js

   # Ou manualmente
   $ mv oldname.js newname.js
   $ git add oldname.js newname.js

O Git detetará a mudança de nome:

   $ git status
   # renamed:    oldname.js -> newname.js

========================================================================================================================================================================================
>BOAS PRÁTICAS:

   * Rever antes de fazer commit:
      $ git diff --staged  # Ver todas as mudanças
      $ git status         # Ver em quais ficheiros
     
   * Fazer commits atómicos (cada um com uma mudança lógica):
      # Exemplo:
      $ git add auth.js
      $ git commit -m "Add authentication logic"
      
      $ git add auth.test.js
      $ git commit -m "Add auth tests"

   * Não preparar código de debugging:
      Usar `git add -p` para não adicionar console.log statements
   
   * Utilizar o staging como um passo de revisão:
      $ git status                    # O que alterou?
      $ git diff                      # Revever mudanças
      $ git add specific-file         # Adicionar com intenção
      $ git diff --staged             # Verificar o que está staged
      $ git commit -m "Mensagem clara"

   * Se adicionou algo que não devia:

      $ git add wrong-file.js
      # Remover do staging:
      $ git restore --staged wrong-file.js

>BOAS PRÁTICAS PARA MENSAGENS DE COMMIT

1. USAR O MODO IMPERATIVO
Escreva como se estivesse a dar uma ordem:

✅ Bom:
   "Adicionar autenticação de utilizador"
   ​​"Corrigir fuga de memória no *parser*"
   "Atualizar dependências para as versões mais recentes"

❌ Mau:
   "Adicionada autenticação de utilizador"
   ​"Corrige fuga de memória"
   "Dependências atualizadas"

Pense: "Este *commit* vai... [a sua mensagem]"

2. SEJA ESPECÍFICO

✅ Bom:
   "Corrigir *null pointer exception* em UserService.getUser()"
   "Adicionar validação de e-mail ao formulário de registo"

❌ Mau:
   "Corrigir erro"
   "Atualizar código"
   "Alterações"

3. EXPLICAR O "PORQUÊ" E NÃO APENAS O "QUÊ"
   
   Adicionar limitação de taxa (*rate limiting*) aos *endpoints* da API
   
   Os utilizadores estavam a encontrar erros 503 durante picos de tráfego.
   A limitação de taxa evita a sobrecarga do servidor ao limitar
   os pedidos a 100/minuto por endereço IP.

4. FAZER REFERÊNCIA A PROBLEMAS

   Corrigir ciclo de redirecionamento no *login*
   
   Os utilizadores ficavam presos num ciclo de redirecionamento quando a sessão expirava.
   Agora, os *cookies* são devidamente limpos antes do redirecionamento.
   
   Corrige #234
   Fecha #235

========================================================================================================================================================================================   
>VER COMMITS

   # Full log
   $ git log
   
   # Ver uma linha por commit
   $ git log --oneline
   
   # Mostrar diffs
   $ git log -p
   
   # Mostrar os últimos 3 commits
   $ git log -3
   
   # Mostrar stats (files changed)
   $ git log --stat
   
   # Formato 'bonito'
   $ git log --pretty=format:"%h %an %ar - %s"
   
   # Histórico do branch em formato gráfico
   $ git log --oneline --graph --all  


>COMMITS VAZIOS

Por vezes, é necessário um commit sem alterações em ficheiros:

   $ git commit --allow-empty -m "Acionar compilação de CI"

Casos de utilização:

   * Acionar pipelines de CI/CD
   * Marcar marcos (milestones)
   * Commits que contêm apenas documentação


>ASSINAR COMMITS

Por motivos de segurança, pode assinar commits com GPG:
   
   # Configurar a chave GPG
   $ git config --global user.signingkey ID_DA_SUA_CHAVE
   
   # Assinar um commit
   $ git commit -S -m "Commit assinado"
   
   # Assinar sempre os commits
   $ git config --global commit.gpgsign true

>MODELOS DE COMMITS

É possível criar um modelo para obter mensagens consistentes:

   # Criar o ficheiro de modelo
   $ cat > ~/.gitmessage << 'EOF'

   # Título: Resumo, no imperativo, 50 caracteres ou menos

   # Corpo: Explique o *quê* e o *porquê* (não o *como*). Limite a 72 caracteres por linha.

   # Referências a *issues*:
   # Resolve #
   $ EOF

   # Configurar o Git para utilizar o modelo
   $ git config --global commit.template ~/.gitmessage

========================================================================================================================================================================================
VIZUALISAR O HISTÓRICO DE COMMITS   

O Git mantém um registo completo de todas as alterações feitas no seu projeto. 
Aprender a navegar e a explorar este histórico é essencial para compreender a base de código e localizar problemas.

   # Comando principal para visualizar o histórico de commits:
   $ git log

   # Visualizar em formato de uma linha:
   $ git log --oneline

   # Em formato de gráfico:
   $ git log --oneline --graph

   # Incluir todos os branches:
   $ git log --oneline --graph --all

   # Formato customizado:
   $ git log --pretty=format:"%h %an %ar - %s"

      Legenda:
         %H	   Full commit hash
         %h	   Abbreviated hash
         %an	Author name
         %ae	Author email
         %ar	Author date, relative
         %ad	Author date
         %s	   Subject (first line)
         %b	   Body

##############################################
LIMITAR O OUTPUT DO LOG

   Por Número:
      $ git log -5              # Últimos 5 commits
      $ git log -n 10           # Últimos 10 commits

   Por Data:
      $ git log --since="2025-01-01"
      $ git log --after="2 weeks ago"
      $ git log --until="2025-01-15"
      $ git log --before="yesterday"

   Por Autor:
      $ git log --author="Jane"
      $ git log --author="jane@example.com"

   Por Conteúdo da Mensagem:
      $ git log --grep="bug"
      $ git log --grep="fix" --grep="auth" --all-match  # Ambos os termos

   Por Alterações em Ficheiros:
      $ git log -- path/to/file.js
      $ git log -- src/

   Por Alterações no Conteúdo:
      # Encontrar commits que alteraram uma cadeia de caracteres específica
      $ git log -S "functionName"

      # Encontrar commits que correspondem a uma expressão regular (regex)
      $ git log -G "function.*validate"

#####################################################################################     
VER OS DETALHES DE UM COMMIT ESPECÍFICO

   # Mostrar o commit mais recente
   $ git show

   # Mostrar um commit específico
   $ git show a1b2c3d

   # Mostrar apenas a mensagem
   $ git show -s a1b2c3d

   # Mostrar apenas as estatísticas
   $ git show --stat a1b2c3d

   # Mostrar um ficheiro específico de um *commit*
   $ git show a1b2c3d:caminho/para/o/ficheiro.js

   # Comparar dois *commits*
   $ git diff a1b2c3d b2c3d4e

   # Comparar um *commit* com o estado atual
   $ git diff a1b2c3d

   # Mostrar apenas os nomes dos ficheiros
   $ git diff --name-only a1b2c3d b2c3d4e

   # Mostrar estatísticas
   $ git diff --stat a1b2c3d b2c3d4e

#####################################################################################    
BLAME: ENCONTRAR QUEM MUDOU O QUÊ

O comando `git blame` mostra quem modificou, pela última vez, cada linha:
   $ git blame path/to/file.js

Resultado:

   a1b2c3d4 (Jane Doe  2025-01-15 10:30:00 -0500  1) function login(user) {
   9876543a (John Smith 2025-01-14 15:45:00 -0500  2)   validateInput(user);
   a1b2c3d4 (Jane Doe  2025-01-15 10:30:00 -0500  3)   return authenticate(user);
   9876543a (John Smith 2025-01-14 15:45:00 -0500  4) }

Opções do `git blame`
   
   # Intervalo de linhas específico
   $ git blame -L 10,20 file.js
   
   # Mostrar o e-mail em vez do nome
   $ git blame -e file.js
   
   # Ignorar alterações em espaços em branco
   $ git blame -w file.js


========================================================================================================================================================================================
ANULAR ALTERAÇÕES

Toda a gente comete erros. 
O Git disponibiliza várias formas de anular alterações, desde correções simples até reversões completas. 

O Kit de Ferramentas de Anulação:

   * Remover um ficheiro da *staging area*	         git restore --staged
   * Descartar alterações no diretório de trabalho	   git restore
   * Modificar o último *commit*	                     git commit --amend
   * Anular *commits* (mantendo as alterações)	      git reset --soft
   * Anular *commits* (perdendo as alterações)	      git reset --hard
   * Anular um *commit* já enviado (*pushed*)	      git revert

#####################################################################################   
DESCARTAR ALTERAÇÕES NO DIRETÓRIO DE TRABALHO

Quando pretender descartar alterações que ainda não foram preparadas (staged):
   
   # Descartar alterações num ficheiro específico
   $ git restore filename.js
   
   # Descartar todas as alterações no diretório de trabalho
   $ git restore .
   
   # Sintaxe mais antiga (ainda funciona)
   $ git checkout -- filename.js

!! AVISO: isto descarta as suas alterações de forma permanente!

#####################################################################################   
RESTARURAR UM FICHEIRO A PARTIR DE UM COMMIT ESPECÍFICO
   
   # Obter ficheiro do commit anterior
   $ git restore --source HEAD~1 filename.js
   
   # Obter ficheiro de um commit específico
   $ git restore --source abc123 filename.js

#####################################################################################   
REMOVER FICHEIROS DA STAGING AREA

   # Remover um ficheiro específico da staging area (mantém as alterações no diretório de trabalho)
   $ git restore --staged filename.js
   
   # Remover todos os ficheiros da staging area
   $ git restore --staged .

#####################################################################################  
MODIFICAR O ÚLTIMO COMMIT

>Corrigir a Mensagem do Commit
   $ git commit --amend -m "Nova mensagem, melhor"

>Adicionar Ficheiros Esquecidos
   $ git add ficheiro-esquecido.js
   $ git commit --amend --no-edit

>Adicionar Alterações Esquecidas

   # Efetue as alterações
   $ echo "fix" >> ficheiro.js

   # Adicione-as à *staging area*
   $ git add ficheiro.js

   # Modifique o *commit* (abre o editor; em alternativa, adicione --no-edit)
   $ git commit --amend

!! IMPORTANTE: Apenas corrige commits que não foram pushed!

#####################################################################################  
ENTENDER O GIT RESET

O comando `git reset` desloca o ponteiro da *branch* e, opcionalmente, altera a *staging area* e o diretório de trabalho.

Três Modos de *Reset*
   
                       Desloca   Limpa     Limpa
                       HEAD      *Stage*   Dir. de Trabalho
                       ─────     ─────     ───────────────
   --soft                ✓         ✗         ✗
   --mixed (default)     ✓         ✓         ✗
   --hard                ✓         ✓         ✓

>SOFT RESET
Desloca o HEAD, mas mantém tudo na *staging area*:

   $ git reset --soft HEAD~1

      Antes:
      HEAD → C → B → A
      
      Depois:
      HEAD → B → A
      (As alterações de C estão agora na *staging area*)

   Caso de uso: Combinar vários *commits* num só.

>MIXED RESET (DEFAULT)
Desloca o HEAD e remove as alterações da *staging area*:

   $ git reset HEAD~1
   # O mesmo que: git reset --mixed HEAD~1

      Antes:
      HEAD → C → B → A
      
      Depois:
      HEAD → B → A
      (As alterações de C encontram-se no diretório de trabalho, fora da *staging area*)

   Caso de uso: Refazer os *commits* de uma forma diferente.
   
>HARD RESET
Desloca o HEAD e descarta todas as alterações:

   $ git reset --hard HEAD~1

      Antes:
      HEAD → C → B → A
      
      Depois:
      HEAD → B → A
      (As alterações de C DESAPARECERAM)

   !! AVISO: Esta operação é destrutiva! As alterações perdem-se.
   
   Caso de uso: Descartar completamente *commits* recentes.

>RESET PARA UM COMMIT ESPECÍFICO

   # Reset para um commit específico
   $ git reset --hard abc123

   # Reset para a origem/main
   $ git reset --hard origin/main

#####################################################################################  
ENTENDER O GIT REVERT

Ao contrário do `reset`, o `revert` cria um novo *commit* que anula as alterações. 
É uma operação segura para *commits* que já foram enviados (*pushed*).

   # Anular o último commit
   $ git revert HEAD

   # Anular um commit específico
   $ git revert abc123

   # Anular sem criar um commit automático (apenas prepara as alterações na *staging area*)
   $ git revert --no-commit abc123

>REVERT VS. RESET

   Reset (reescreve o histórico):
      A → B → C → D
      ↓
      A → B → C (o D desaparece)


   Revert (preserva o histórico):
      A → B → C → D
      ↓
      A → B → C → D → D' (o D' anula o D)

   
         Aspeto	               Reset	Revert
   Modifica o histórico	         Sim	Não
   Seguro para commits enviados	Não	Sim
   Cria um novo commit	         Não	Sim

#####################################################################################  
RECUPERAR DE ERROS - O REFLOG

O Git mantém um registo de todos os movimentos do HEAD:
   $ git reflog

   Output:
   
   abc123 HEAD@{0}: reset: a mover para HEAD~3
   def456 HEAD@{1}: commit: Adicionar funcionalidade
   789abc HEAD@{2}: commit: Corrigir erro

>RECUPERAR UM COMMIT "PERDIDO"

 Se, por engano, tiver recuado demasiado com um `reset`:

   # Encontrar o commit perdido no reflog
   $ git reflog

   # Voltar a esse ponto com um reset
   $ git reset --hard abc123
   
   # Ou criar uma nova branch a apontar para esse commit
   $ git branch recovered abc123

O reflog mantém registos durante cerca de 90 dias.

#####################################################################################  
LIMPEZA DE FICHEIROS NÃO RASTREADOS

É possível remover ficheiros não rastreados pelo Git:

   # Ver o que seria removido (simulação)
   $ git clean -n

   # Remover ficheiros não rastreados
   $ git clean -f

   # Remover ficheiros e diretórios não rastreados
   $ git clean -fd

   # Remover também ficheiros ignorados
   $ git clean -fdx

!! AVISO: o git clean elimina estes ficheiros permanentemente!

#####################################################################################  
CENÁRIOS COMUNS

>"Fiz um *commit* na *branch* errada"

   # Guarda o *hash* do *commit*
   $ git log -1  # Nota: abc123

   # Faz *reset* nesta *branch*
   $ git reset --hard HEAD~1

   # Muda para a *branch* correta
   $ git checkout correct-branch

   # Aplica o *commit* específico (*cherry-pick*)
   $ git cherry-pick abc123

>"Preciso de desfazer os últimos 3 *commits*, mas manter as alterações"

   $ git reset --soft HEAD~3
   # As alterações dos 3 *commits* estão agora na *staging area*
   # Podes voltar a fazer *commit* das mesmas de forma diferente

>"Fiz *push* de um *commit* incorreto"

   # Cria um *commit* que anula o anterior
   $ git revert abc123
   
   # Faz *push* da anulação
   $ git push

>"Apaguei um ficheiro por engano"

   # Se não estiver na *staging area*
   $ git restore deleted-file.js
   
   # Se já tiver sido feito *commit*
   $ git checkout HEAD~1 -- deleted-file.js

#####################################################################################  
RESUMO

* Usa `git restore` para descartar alterações no diretório de trabalho
* Usa `git restore --staged` para remover ficheiros da *staging area*
* Usa `git commit --amend` para corrigir o último *commit*
* Usa `git reset` para mover o indicador da *branch* (`soft`/`mixed`/`hard`)
* Usa `git revert` para *commits* já enviados (*pushed*) —> isto cria um novo *commit*
* Usa `git reflog` para recuperar de erros de reversão
* Usa `git clean` para remover ficheiros não rastreados (*untracked*)

* Usa comandos destrutivos (`--hard`, `clean`) apenas quando tiveres a certeza!!

========================================================================================================================================================================================
BRANCHING

Um branch é, simplesmente, um ponteiro para um *commit*. 
Quando cria um branch, o Git cria um novo indicado — não copia quaisquer ficheiros:

   main:      A ← B ← C ← D
                          ↑
                         main (pointer)

O branch predefinido (normalmente `main` ou `master`) é apenas uma ramificação como qualquer outra.

Porquê usar branches:
   * Isolamento	               Trabalhar em funcionalidades sem afetar o código estável
   * Desenvolvimento paralelo	   Desenvolvimento de várias funcionalidades em simultâneo
   * Experimentação	            Testar ideias sem riscos
   * Revisão de código	         Rever alterações antes da integração
   * Lançamentos	               Manter diferentes versões

#####################################################################################  
COMO FUNCIONAM OS BRANCHES

* Antes de criar um Branch:

   A ← B ← C ← D
               ↑
               main
               HEAD

   - O HEAD aponta para o branch atual (main), que aponta para o *commit* D.


* Após Ccriar um Branch:

   A ← B ← C ← D
               ↑
               main
               feature ← HEAD

   - Tanto a branch *main* como a *feature* apontam para o mesmo *commit* (D).


* Após efetuar commits na feature

   A ← B ← C ← D
               ↑ ↖
               main  E ← F
                        ↑
                        feature
                        HEAD

   - A branch *feature* avançou com os novos commits E e F. A *main* permanece onde estava.

##################################################################################### 
O HEAD

O HEAD é um indicador especial que indica a sua posição atual no repositório:

   * Normalmente, aponta para o nome de uma *branch* (p. ex., HEAD → main → *commit* D)
   * Ao efetuar um *commit*, a *branch* atual avança
   * Ao fazer *checkout* de uma *branch*, o HEAD desloca-se para essa *branch*

HEAD Desacoplado (*Detached HEAD*):
Se o HEAD apontar diretamente para um *commit* (e não para uma *branch*), encontra-se no estado de "HEAD desacoplado" (*detached HEAD*):

      $ git checkout abc1234  # HEAD desacoplado

Isto é útil para consultar *commits* antigos, mas tenha cuidado: os *commits* criados nesta situação poderão perder-se se não criar uma *branch*.

##################################################################################### 
VISUALIZAR BRANCHES

   # Ver todos os branches
   $ git branch
   
   # Ver branches com o último *commit*
   $ git branch -v
   
   # Ver todos os branches, incluindo remotos
   $ git branch -a
   
   # Registo visual dos branches
   $ git log --oneline --graph --all

Resultado de: 
   $ git log --oneline --graph --all:

      * 7d3f8e2 (HEAD -> feature) Adicionar formulário de início de sessão
      * 5b2c1a0 Iniciar funcionalidade de início de sessão
      | * 9c4d3b2 (main) Corrigir erro na página inicial
      |/
      * 1a2b3c4 *Commit* inicial

##################################################################################### 
NOMENCLATURA DE BRANCHES

Os *branches* podem ter praticamente qualquer nome, mas devem seguir estas convenções:

Prefixos Comuns:
   
   * feature/	   Novas funcionalidades
   * bugfix/	   Correções de erros
   * hotfix/	   Correções urgentes em produção
   * release/	   Preparação de uma versão (release)
   * docs/	      Documentação
   * test/	      Testes

##################################################################################### 
OS BRANCHES SÃO BARATOS
Ao contrário de alguns sistemas de controlo de versões, os *branches* no Git são:

   * Rápidos de criar: Apenas criam um ficheiro de 41 bytes (um ponteiro)
   * Rápidos de alternar: Apenas atualizam alguns ponteiros
   * Eficientes em termos de espaço: Não duplicam ficheiros

##################################################################################### 
FLUXOS DE TRABALHOS COMUNS COM BRANCHES

Ramo de funcionalidade simples:
   
   main:     A ── B ── C ────────── M
                   \               /
   feature:         D ── E ── F ──

   1 - Criar um ramo a partir de `main`
   2 - Desenvolver a funcionalidade
   3 - Integrar (*merge*) de volta em `main`


Ramos de longa duração:

   main:       ○ ── ○ ── ○ ── ○ ── ○ ── ○
                  \           / \
   develop:        ○ ── ○ ── ○   ○ ── ○
                     \       /
   feature:           ○ ── ○

   * `main` está sempre estável/em produção
   * `develop` serve para integração
   * Utilizar `feature` para trabalho específico

========================================================================================================================================================================================
CRIAR E MUDAR BRANCHES

CRIAÇÃO

$ git branch

   # Cria um novo branch:
   $ git branch feature-login

Isto cria o branch `feature-login` mas não muda para ele. Mantém o HEAD no branch corrente:
   
        A ← B ← C ← D
                    ↑
                   main ← HEAD
                   feature-login

$ git checkout -b nome_branch  (Formato clássico)

   # Cria e muda de branch num único comando
   $ git checkout -b feature-login

$ git switch -c nome_branch     (Formato moderno)

   # Cria e muda de branch num único comando
   $ git switch -c feature-login

Ambos fazem a mesma coisa - criam um branch e mudam para ele

##################################################################################### 
MUDANÇA DE BRANCHES

$ git checkout   (Formato clássico)

   $ git checkout feature-login   

$ git switch     (Formato moderno)

   $ git switch feature-login 


Porquê `switch`?
   O `git checkout` tem muitas funções (mudar de branches, restaurar ficheiros, etc).
   O `git switch` é dedicado à mudança de branches, tornando o comando mais claro e seguro.

   # Ambos fazem a mesma coisa:
      $ git checkout main
      $ git switch main

   # Mas apenas checkout consegue restaurar ficheiros:
      $ git checkout -- file.js    # Restora ficheiro
      # git switch -- file.js      # Não funciona
   
##################################################################################### 
CRIAR BRANCHES A PARTIR DE PONTOS DIFERENTES

* A partir de outro branch:

   # Criar um branch a partir do 'main' (estando em qualquer branch)
   $ git branch feature-x main

   # Criar e mudar para o ramo
   $ git switch -c feature-x main

* A partir de um 'Commit' específico:

   # Criar um branch a partir de um 'commit'
   $ git branch bugfix abc123

   # Criar e mudar para branch
   $ git switch -c bugfix abc123

* A partir de uma 'Tag'

     $ git switch -c hotfix v1.0.0

##################################################################################### 
LISTAR BRANCHES

   # Lista branches locais
   $ git branch
   
   # Listar com mais informação
   $ git branch -v
   
   # Listar todos os branches (incluindo remotos)
   $ git branch -a
   
   # Listar apenas branches remotos
   $ git branch -r
   
   # Listar branches que contêm um commit específico
   $ git branch --contains abc123
   
   # Listar branches integrados
   $ git branch --merged
   
   # Listar branches não integrados
   $ git branch --no-merged

##################################################################################### 
RENOMEAR BRANCHES

   # Renomear a branch atual
   $ git branch -m novo-nome

   # Renomear uma branch específica
   $ git branch -m nome-antigo novo-nome

   # Forçar a renomeação (mesmo que o novo nome já exista)
   $ git branch -M novo-nome

>RENOMEAR A BRANCH PREDEFINIDA

   # Renomear 'master' para 'main'
   $ git branch -m master main
   
   # Atualizar o repositório remoto (se aplicável)
   $ git push -u origin main
   $ git push origin --delete master

##################################################################################### 
ELIMINAR BRANCHES

>ELIMINAR FUSED BRANCH

   $ git branch -d feature-login

O Git recusa a operação se a branch não estiver fundida:

   error: The branch 'feature-login' is not fully merged.
   If you are sure you want to delete it, run 'git branch -D feature-login'.

>ELIMINAÇÃO FORÇADA

   $ git branch -D feature-login

>ELIMINAR BRANCH REMOTA

   $ git push origin --delete feature-login
   # ou
   $ git push origin :feature-login

##################################################################################### 
MUDAR DE BRANCH COM ALTERAÇÕES NÂO SUBMETIDAS

Se tiver alterações não submetidas e tentar mudar de ramo:

>CASO 1: SEM CONFLITO
Se as suas alterações não entrarem em conflito com o ramo de destino, o Git transporta-as consigo:

   # Efetuar alterações no ficheiro file.js
   $ git switch other-branch
   # As alterações acompanham-no

>CASO 2: CONFLITO
Se as alterações corressem o risco de ser sobrescritas, o Git impede a mudança de ramo:

   error: Your local changes to the following files would be overwritten by checkout:
        file.js
   Please commit your changes or stash them before you switch branches.

>SOLUÇÕES:

   # Opção 1: Fazer commit das alterações
   $ git commit -am "WIP: guardar trabalho"
   $ git switch other-branch

   # Opção 2: Fazer stash das alterações
   $ git stash
   $ git switch other-branch
   $ git stash pop  # Mais tarde, restaurar as alterações
   
   # Opção 3: Descartar as alterações (cuidado!)
   $ git restore .
   $ git switch other-branch

##################################################################################### 
DETACHED HEAD STATE

Por vezes, é necessário consultar um *commit* antigo:
   $ git checkout abc123

Verá a seguinte mensagem:

   You are in 'detached HEAD' state. You can look around, make experimental
   changes and commit them, and you can discard any commits you make in this
   state without impacting any branches by switching back to a branch.

>TRABALHAR EM ESTADO 'DETACHED HEAD'

Pode efetuar *commits*, mas estes não estarão associados a nenhuma *branch*:

   $ git checkout abc123
   # Efetuar alterações
   $ git commit -m "Alteração experimental"
   # Este *commit* fica "órfão"

>PRESERVAR COMMITS EM ESTADO 'DETACHED HEAD'

Se pretender manter os *commits* realizados enquanto estava em 'detached HEAD':
   
   # Enquanto ainda estiver em 'detached HEAD'
   $ git switch -c my-experiment
   # Agora, os seus *commits* fazem parte de uma *branch*

Ou, caso já tenha mudado para outra referência:

   # Localizar o *commit* no *reflog*
   $ git reflog
   # Criar uma *branch* que aponte para esse *commit*
   $ git branch rescue abc123

========================================================================================================================================================================================
BRANCH MERGE

Depois de desenvolver uma funcionalidade numa ramificação, quererá integrá-la novamente na sua ramificação principal. 
A isto chama-se fusão (merge).

>O QUE É O MERGE?

   A fusão combina as alterações de uma ramificação noutra:
   
   Antes da fusão:
      main:      A ← B ← C
                          \
      feature:             D ← E
   
   Após a fusão:
      main:      A ← B ← C ← ← ← M
                          \     /
      feature:             D ← E

##################################################################################### 
TIPOS DE MERGES

>FAST-FORWARD MERGE
Quando a *branch* de destino não tem novos *commits* desde que a *branch* de origem se separou:

   Antes:
      main:      A ← B ← C
                         ↑
                         main
      feature:           D ← E
                         ↑
                         feature
   
   Depois (*fast-forward*):
      main:      A ← B ← C ← D ← E
                                 ↑
                                 main
                                 feature

- O Git apenas avança o ponteiro da *main*. Não é necessário um *commit* de fusão (*merge commit*).

   $ git switch main
   $ git merge feature
   # Fusão fast-forward

   Output:
   
   Updating abc123..def456
   Fast-forward
    login.js | 25 ++++++++++++++++
    1 file changed, 25 insertions(+)

>THREE-WAY MERGE
Quando ambos os *branches* têm *commits*:

   Antes:
      main:      A ← B ← C ← F
                         \
      feature:            D ← E
   
   Depois:
      main:      A ← B ← C ← F ← M
                         \     /
      feature:            D ← E

- O Git cria um commit merge (M) com dois branches pai.

   $ git switch main
   $ git merge feature
   # Three-way merge, creates merge commit
  
##################################################################################### 
EFETUAR MERGES

>MERGE BÁSICO

   # Mudar para o branch de destino
   $ git switch main

   # Fundir o branch de origem
   $ git merge feature

   # O branch 'feature' ainda existe
   $ git branch -d feature  # Pode-se eliminar se concluída

>MERGE COM MENSAGEM PERSONALIZADA

   $ git merge feature -m "Merge feature branch: adicionar início de sessão de utilizador"

>SEM FAST-FORWARD
   Forçar um merge de um *commit*, mesmo quando o fast-forward é possível:

   $ git merge --no-ff feature

- Isto preserva o histórico dos branches:

  Com --no-ff:
  
   main:      A ← B ← C ← ← ← M
                      \     /
   feature:            D ← E


  Sem --no-ff (fast-forward):
  
   main:      A ← B ← C ← D ← E
                            ↑
                          (feature history lost)

   
>SQUASH MERGE
   Combinar todos os *commits* num único *commit*:

   $ git merge --squash feature
   $ git commit -m "Adicionar funcionalidade de início de sessão"

   Antes:
      feature:  D ← E ← F (3 *commits*)
   
   Após squash merge:
      main:     ... ← X (um único *commit* com todas as alterações)

- Os *commits* da ramificação 'feature' são combinados. O histórico original perde-se na ramificação 'main'.

>OPÇÔES DE MERGING

--no-ff	      Cria sempre um *commit* de fusão
--ff-only	   Realiza a fusão apenas se for possível um avanço rápido (*fast-forward*)
--squash	      Combina num único *commit*
--no-commit	   Realiza a fusão, mas não efetua o *commit* automaticamente
--abort	      Cancela uma fusão em curso

##################################################################################### 
ENTENDER MERGE COMMITS

Um *commit* de *merge* tem dois (ou mais) pais:

   $ git log --oneline
   # abc123 Merge branch 'feature' into main
   # def456 Add login form (from feature)
   # 789abc Update homepage (from main)

   git show abc123
   # Mostra o *merge commit* com ambos os pais

##################################################################################### 
ESTRATÉGIAS DE MERGING

O Git usa diferentes estratégias dependendo da situação:

>RRECURSIVA (PADRÃO)
   Usada para *three-way merges*. Lida bem com a maioria dos casos.
   
   $ git merge -s recursive feature

>OURS
   Mantém a 'nossa' versão de tudo (descartas alterações de outros):

   $ git merge -s ours feature

>OCTOPUS
   Faz merge de vários branches de uma só vez:
   
   $ git merge feature1 feature2 feature3
   # Faz merge dos branches indicados

##################################################################################### 
VISUALIZAR O HISTÓRICO DE MERGES

   # Ver commits de fusão
   $ git log --merges
   
   # Ver commits que não são de fusão
   $ git log --no-merges
   
   # Visualização gráfica
   $ git log --oneline --graph
   
   # Apenas o primeiro progenitor (histórico da ramificação principal)
   $ git log --first-parent
   
##################################################################################### 
CANCELAR UM MERGE

Se algo correr mal durante um merge:
   
   $ git merge --abort

##################################################################################### 
BOAS PRÁTICAS DE MERGING

1. Atualizar antes de fazer *merge*:

   $ git switch main
   $ git pull                    # Obter as alterações mais recentes
   $ git switch feature
   $ git merge main              # Atualizar 'feature' com as alterações da 'main'
   # Resolver eventuais conflitos
   $ git switch main
   $ git merge feature           # Agora, fundir a 'feature' na 'main'


2. Utilizar --no-ff (no fast-foward) para Feature Branches

   $ git merge --no-ff feature -m "Merge feature: user authentication"

- Preserva o facto de uma funcionalidade ter sido desenvolvida num branch e não no main.


3. Eliminar *merged branches*:

   $ git branch -d feature

- Mantem a tua lista de ramos organizada.


4. Testar Antes de efetuar *merge*
   Verifique sempre se a fusão não compromete o funcionamento:

   $ git merge feature
   $ npm test                    # Executar testes
   $ npm run build               # Verificar a build

========================================================================================================================================================================================
RESOLVER CONFLITOS DE MERGING

Os conflitos de *merging* ocorrem quando o Git não consegue combinar automaticamente alterações provenientes de diferentes *branches*. 
Isto acontece quando as mesmas linhas de código são modificadas de formas diferentes em ambos os *branches*.

>QUANDO ACONTECEM CONFLITOS?
   * As mesmas linhas de código são alteradas em ambas as *branches*
   * Um ficheiro é modificado numa *branch* e apagado noutra
   * O mesmo ficheiro é adicionado em ambas as branches contendo conteúdo diferente

Exemplo:
   main:      Changed line 5 to "Hello World"
   feature:   Changed line 5 to "Hello Git"
   
   Git: "I don't know which one you want!"
   
##################################################################################### 
ANATOMIA DE UM CONFLITO

Quando um conflito ocorre, o Git marca as seções em conflito:

   function greet() {
   <<<<<<< HEAD
     return "Hello World";
   =======
     return "Hello Git";
   >>>>>>> feature
   }

MARCADOR	            SIGNIFICADO
-----------------------------------------------------------
<<<<<<< HEAD	   | Começo da versão do teu *branch* atual
=======	         | Separador entre versões
>>>>>>> feature   | Fim, mostra o nome do branch a ser integrado (merged)

- Tudo entre: 
      <<<<<<< 
         e 
      =======
pertence à TUA versão (neste exemplo, MAIN). 

- Tudo entre: 
      ======= 
         e 
      >>>>>>>
pertence à OUTRA versão (neste exemplo, FEATURE).

##################################################################################### 
PROCESSO DE RESOLUÇÃO DE UM CONFLITO

1. TENTAR EFETUAR O MERGE

   $ git merge feature

- Se houverem conflitos, o output deverá ser algo do género:
   
   Auto-merging file.js
   CONFLICT (content): Merge conflict in file.js
   Automatic merge failed; fix conflicts and then commit the result.

2. VERIFICAR O ESTADO

   $ git status

OUTPUT:
   On branch main
   You have unmerged paths.
     (fix conflicts and run "git commit")
   
   Unmerged paths:
     (use "git add <file>..." to mark resolution)
           both modified:   file.js

3. ABRIR O FICHEIRO EM CONFLITO

Abrir e editar o ficheiro para resolver conflitos:

      // Antes da resolução:
      function greet() {
      <<<<<<< HEAD
        return "Hello World";
      =======
        return "Hello Git";
      >>>>>>> feature
      }
      
      // Após resolução (escolhes o que manter):
      function greet() {
        return "Hello Git World";  // Neste caso combinou-se ambos
      }

4. MARCAR COMO RESOLVIDO

   $ git add file.js

5. COMPLETAR O MERGE

   $ git commit
   # Git gera automaticamente uma mensagem de commit do merge

Ou com uma mensagem customizada:

   $ git commit -m "Merge feature: resolver conflito de saudação"


##################################################################################### 
ESTRATÉGIAS DE RESOLUÇÃO

>MANTER A NOSSA

Manter a nossa versão, descartar a dos outros:

   // Remover a versão deles e os marcadores de conflito
   function greet() {
     return "Hello World";
   }

>MANTER A DELES

Manter a versão deles, descartar a nossa:

   function greet() {
     return "Hello Git";
   }

>COMBINAR AMBAS

Misturar a lógica de ambas:

   function greet() {
     return "Hello Git World";
   }

>SOBRESCREVER COMPLETAMENTE

Por vezes, nenhuma versão está correta:

   function greet(name) {
     return `Hello ${name}`;
   }

##################################################################################### 
USAR FERRAMENTAS DE MERGE

>Ferramenta integrada de *merge*

   $ git mergetool

Faz como que o Git abra a sua ferramenta de *merge* configura.

>Configurar uma ferramenta de *merge*

   # Usar VS Code
   $ git config --global merge.tool vscode
   $ git config --global mergetool.vscode.cmd 'code --wait $MERGED'

   # Usar vimdiff
   $ git config --global merge.tool vimdiff

>Resolver problemas de conflito no VS Code:

O VS Code mostra marcadores de conflito, com botões úteis:
   * "Aceitar a alteração atual" (a sua)
   * "Aceitar a alteração recebida" (a deles)
   * "Aceitar ambas as alterações"
   * "Comparar alterações"

##################################################################################### 
ABORTAR UM MERGE

Se quiser desistir do merge:

   $ git merge --abort

Isto devolve a tua *branch* ao estado em que se encontrava antes do *merge* ter começado.

##################################################################################### 
PREVENIR CONFLITOS

1. Comunique com a sua equipa:

- Coordene quem está a trabalhar em que ficheiros.

2. Faça *pull* frequentemente:

   $ git pull origin main

- Mantenha-se a par das alterações.

3. Faça alterações mais pequenas e focadas:

- Alterações extensas que afetam muitos ficheiros aumentam a probabilidade de conflitos.

4. Integre a *main* na sua *branch* regularmente:

   $ git switch feature
   $ git merge main  # Resolva os conflitos enquanto ainda são pequenos


#####################################################################################
CENÀRIOS DE CONFLITO COMPLEXOS

>FICHEIRO ELIMINADO NUM DOS BRANCHES

   OUTPUT:
   CONFLICT (modify/delete): file.js deleted in feature and modified in HEAD.

Opções:
   
   # Manter o ficheiro
   $ git add file.js
   
   # Eliminar o ficheiro
   $ git rm file.js

>FICHEIRO ADICIONADO EM AMBOS OS BRANCHES

   OUTPUT:
   CONFLICT (add/add): Merge conflict in config.js

Ambos os ramos adicionaram o mesmo ficheiro com conteúdos diferentes. 
Resolva tal como um conflito de conteúdo normal.

>CONFLITOS EM FICHEIROS BINÁRIOS

Ficheiros binários (imagens, etc.) não podem ser *merged* ao nível do texto:

   # Manter a nossa versão
   $ git checkout --ours image.png
   $ git add image.png
   
   # Manter a versão deles
   $ git checkout --theirs image.png
   $ git add image.png


#####################################################################################
USAR GIT CHECKOUT EM CONFLITOS

Durante conflitos, podes escolher as versões:

   # Usar a nossa versão por inteiro
   $ git checkout --ours file.js
   
   # Usar a versão deles por inteiro
   $ git checkout --theirs file.js
   
   # E marcar como resolvido
   $ git add file.js

#####################################################################################
VERIFICAR ESTADO DE CONFLITOS

   # Ver ficheiros em conflito
   $ git diff --name-only --diff-filter=U
   
   # Ver detalhes do conflito
   $ git diff
   
   # Ver ficheiro específico
   $ git diff file.js

#####################################################################################
SUMÁRIO

* Os conflitos ocorrem quando o Git não consegue fazer *merge* de alterações automaticamente;
* Os marcadores de conflito aparecem em ambas as versões do ficheiro;
* Resolução: editar o ficheiro → git add → git commit;
* Utilize `git merge --abort` para cancelar;
* Utilize `--ours` ou `--theirs` para uma resolução rápida;
* As ferramentas de *merge* facilitam a resolução;
* Previna conflitos efetuando *pull* frequentemente e comunicando;

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

