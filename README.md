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

##############################################     
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


##############################################    
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


##############################################   
DESCARTAR ALTERAÇÕES NO DIRETÓRIO DE TRABALHO

Quando pretender descartar alterações que ainda não foram preparadas (staged):
   
   # Descartar alterações num ficheiro específico
   $ git restore filename.js
   
   # Descartar todas as alterações no diretório de trabalho
   $ git restore .
   
   # Sintaxe mais antiga (ainda funciona)
   $ git checkout -- filename.js

!! AVISO: isto descarta as suas alterações de forma permanente!


##############################################   
RESTARURAR UM FICHEIRO A PARTIR DE UM COMMIT ESPECÍFICO
   
   # Obter ficheiro do commit anterior
   $ git restore --source HEAD~1 filename.js
   
   # Obter ficheiro de um commit específico
   $ git restore --source abc123 filename.js


##############################################   
REMOVER FICHEIROS DA STAGING AREA

   # Remover um ficheiro específico da staging area (mantém as alterações no diretório de trabalho)
   $ git restore --staged filename.js
   
   # Remover todos os ficheiros da staging area
   $ git restore --staged .


##############################################  
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


##############################################  
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

##############################################  
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

##############################################  
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

##############################################  
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

##############################################  
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

##############################################  
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

