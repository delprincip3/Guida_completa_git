# Guida Completa a Git e GitHub - Dai Fondamenti all'Uso Avanzato

## Indice
1. [Introduzione](#introduzione)
2. [Concetti Fondamentali](#concetti-fondamentali)
3. [Configurazione Iniziale](#configurazione-iniziale)
4. [Comandi Git Essenziali](#comandi-git-essenziali)
5. [Lavorare con i Branch](#lavorare-con-i-branch)
6. [Workflow Git Popolari](#workflow-git-popolari)
7. [Configurazione SSH per GitHub](#configurazione-ssh-per-github)
8. [Tecniche Avanzate](#tecniche-avanzate)
9. [Best Practices](#best-practices)
10. [Risoluzione dei Problemi Comuni](#risoluzione-dei-problemi-comuni)

---

## 1. Introduzione

Git è un sistema di controllo versione distribuito che permette di tracciare le modifiche al codice nel tempo e collaborare efficacemente con altri sviluppatori. GitHub è una piattaforma web che ospita repository Git e aggiunge funzionalità collaborative come pull request, issue tracking e molto altro.

### Perché usare Git?
- **Tracciamento delle modifiche**: ogni cambiamento viene registrato con chi, quando e perché
- **Collaborazione**: più persone possono lavorare sullo stesso progetto simultaneamente
- **Backup**: il codice è salvato sia localmente che su server remoti
- **Branching**: possibilità di lavorare su funzionalità isolate senza impattare il codice principale
- **Ripristino**: possibilità di tornare a versioni precedenti del codice

---

## 2. Concetti Fondamentali

### Repository (Repo)
Una cartella che contiene tutti i file del progetto e la cronologia delle modifiche.

### Commit
Un'istantanea del codice in un momento specifico. Ogni commit ha:
- Un ID univoco (hash SHA)
- Autore e timestamp
- Messaggio che descrive le modifiche

### Branch
Una linea di sviluppo indipendente. Il branch principale è solitamente chiamato `main` o `master`.

### Remote
Un repository Git ospitato su un server (come GitHub).

### Le Tre Aree di Git
1. **Working Directory**: dove modifichi i file
2. **Staging Area (Index)**: dove prepari le modifiche per il commit
3. **Repository**: dove sono salvati permanentemente i commit

---

## 3. Configurazione Iniziale

### Installazione
- **Windows**: Scarica Git da [git-scm.com](https://git-scm.com)
- **macOS**: `brew install git` (con Homebrew)
- **Linux**: `sudo apt-get install git` (Ubuntu/Debian) o `sudo yum install git` (Fedora)

### Configurazione Utente
```bash
# Imposta nome e email (obbligatorio)
git config --global user.name "Il Tuo Nome"
git config --global user.email "tua-email@esempio.com"

# Editor preferito (opzionale)
git config --global core.editor "code"  # Per VS Code

# Visualizza tutte le configurazioni
git config --list
```

### Configurazione Alias Utili
```bash
# Alias per comandi frequenti
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.lg "log --oneline --graph --decorate --all"
```

---

## 4. Comandi Git Essenziali

### Inizializzare un Repository

```bash
# Crea un nuovo repository
git init

# Clona un repository esistente
git clone https://github.com/utente/repository.git
git clone git@github.com:utente/repository.git  # Con SSH
```

### Comandi Base per il Workflow Quotidiano

```bash
# 1. Controlla lo stato del repository
git status

# 2. Aggiungi file alla staging area
git add nomefile.txt           # Singolo file
git add .                      # Tutti i file modificati
git add *.js                   # Tutti i file JavaScript
git add cartella/              # Intera cartella

# 3. Crea un commit
git commit -m "Descrizione delle modifiche"

# 4. Visualizza la cronologia
git log                        # Log completo
git log --oneline             # Log compatto
git log --graph               # Con grafico dei branch
git log --author="Nome"       # Commit di un autore specifico

# 5. Sincronizza con il remote
git push origin main          # Invia modifiche al remote
git pull origin main          # Scarica e integra modifiche
git fetch                     # Scarica senza integrare
```

### Gestione dei File

```bash
# Rimuovi file
git rm nomefile.txt           # Rimuove dal repo e dal filesystem
git rm --cached nomefile.txt  # Rimuove solo dal repo

# Rinomina/sposta file
git mv vecchio.txt nuovo.txt

# Ignora file con .gitignore
echo "node_modules/" >> .gitignore
echo "*.log" >> .gitignore
echo ".env" >> .gitignore
```

### Visualizzare le Differenze

```bash
# Differenze non ancora in staging
git diff

# Differenze in staging
git diff --staged

# Differenze tra branch
git diff main feature-branch

# Differenze di un file specifico
git diff nomefile.txt
```

---

## 5. Lavorare con i Branch

### Comandi Branch Fondamentali

```bash
# Crea un nuovo branch
git branch nome-branch

# Passa a un branch
git checkout nome-branch

# Crea e passa a un nuovo branch
git checkout -b nome-branch

# Lista tutti i branch
git branch              # Locali
git branch -r          # Remoti
git branch -a          # Tutti

# Elimina branch
git branch -d nome-branch    # Solo se già mergiato
git branch -D nome-branch    # Forza eliminazione

# Rinomina branch
git branch -m vecchio-nome nuovo-nome
```

### Merge e Gestione Conflitti

```bash
# Merge di un branch nel branch corrente
git checkout main
git merge feature-branch

# In caso di conflitti:
# 1. Apri i file con conflitti
# 2. Cerca i marcatori: <<<<<<< HEAD, =======, >>>>>>> branch-name
# 3. Risolvi manualmente i conflitti
# 4. Aggiungi i file risolti
git add file-risolto.txt
# 5. Completa il merge
git commit
```

---

## 6. Workflow Git Popolari

### GitHub Flow (Semplice e Moderno)
1. Il branch `main` contiene sempre codice pronto per la produzione
2. Crea branch descrittivi per nuove funzionalità: `feature/nuova-funzione`
3. Fai commit regolari e push al remote
4. Apri una Pull Request per la revisione del codice
5. Dopo l'approvazione, fai il merge in `main`
6. Elimina il feature branch

```bash
# Esempio pratico
git checkout -b feature/login-utente
# ... lavora e fai commit ...
git push origin feature/login-utente
# Apri PR su GitHub
# Dopo approvazione e merge, elimina il branch
git branch -d feature/login-utente
```

### GitFlow (Per Progetti con Release Programmate)
- **main**: codice in produzione
- **develop**: integrazione delle feature
- **feature/**: nuove funzionalità
- **release/**: preparazione release
- **hotfix/**: fix urgenti in produzione

### Trunk-Based Development
- Tutti lavorano su un singolo branch (`main`)
- Feature toggle per nascondere funzionalità incomplete
- Integrazione continua essenziale

---

## 7. Configurazione SSH per GitHub

### Generare una Chiave SSH

```bash
# 1. Genera una nuova chiave SSH
ssh-keygen -t ed25519 -C "tua-email@esempio.com"

# Se il sistema non supporta Ed25519
ssh-keygen -t rsa -b 4096 -C "tua-email@esempio.com"

# 2. Premi Enter per accettare il percorso predefinito
# 3. Inserisci una passphrase (opzionale ma consigliata)
```

### Aggiungere la Chiave all'SSH Agent

```bash
# Avvia l'ssh-agent
eval "$(ssh-agent -s)"

# macOS: configura per salvare la passphrase
open ~/.ssh/config
# Aggiungi:
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519

# Aggiungi la chiave all'agent
ssh-add ~/.ssh/id_ed25519
```

### Aggiungere la Chiave a GitHub

```bash
# Copia la chiave pubblica
cat ~/.ssh/id_ed25519.pub
# Su macOS: pbcopy < ~/.ssh/id_ed25519.pub
# Su Linux: xclip -sel clip < ~/.ssh/id_ed25519.pub
```

1. Vai su GitHub → Settings → SSH and GPG keys
2. Clicca "New SSH key"
3. Incolla la chiave e dai un nome descrittivo
4. Testa la connessione: `ssh -T git@github.com`

---

## 8. Tecniche Avanzate

### Interactive Rebase

Il rebase interattivo permette di modificare la storia dei commit:

```bash
# Modifica gli ultimi 3 commit
git rebase -i HEAD~3

# Nell'editor che si apre, puoi:
# pick = usa il commit così com'è
# reword = modifica il messaggio del commit
# edit = modifica il contenuto del commit
# squash = unisci con il commit precedente
# fixup = unisci senza modificare il messaggio
# drop = elimina il commit
```

#### Esempio: Unire Commit
```bash
# Supponiamo di avere:
# abc1234 Aggiungi login
# def5678 Fix typo nel login
# ghi9012 Un altro fix al login

git rebase -i HEAD~3

# Nell'editor:
pick abc1234 Aggiungi login
fixup def5678 Fix typo nel login
fixup ghi9012 Un altro fix al login

# Risultato: un solo commit pulito
```

### Stash - Salvare Modifiche Temporaneamente

```bash
# Salva le modifiche correnti
git stash

# Salva con un messaggio
git stash save "Lavoro in corso su feature X"

# Lista degli stash
git stash list

# Applica l'ultimo stash
git stash pop

# Applica uno stash specifico
git stash apply stash@{1}

# Elimina uno stash
git stash drop stash@{0}
```

### Cherry-pick - Applicare Commit Specifici

```bash
# Applica un commit specifico al branch corrente
git cherry-pick abc1234

# Cherry-pick di più commit
git cherry-pick abc1234 def5678
```

### Reset vs Revert

```bash
# Reset: modifica la storia (usa solo su commit locali)
git reset --soft HEAD~1   # Mantiene le modifiche in staging
git reset --mixed HEAD~1  # Mantiene le modifiche ma non in staging
git reset --hard HEAD~1   # Elimina completamente le modifiche

# Revert: crea un nuovo commit che annulla le modifiche
git revert abc1234        # Più sicuro per commit già pushati
```

### Reflog - Recuperare Commit "Persi"

```bash
# Visualizza tutte le azioni
git reflog

# Recupera un commit
git checkout abc1234
# O crea un branch da quel punto
git checkout -b recovery-branch abc1234
```

---

## 9. Best Practices

### Messaggi di Commit Efficaci

```bash
# Formato consigliato
tipo(ambito): descrizione breve

Descrizione dettagliata del perché delle modifiche.
Può essere su più righe.

Closes #123  # Riferimento a issue

# Esempi:
feat(auth): aggiungi autenticazione OAuth
fix(api): risolvi errore timeout nelle chiamate
docs(readme): aggiorna istruzioni installazione
style(css): formatta file secondo standard
refactor(utils): ottimizza funzione di parsing
test(auth): aggiungi test per login
chore(deps): aggiorna dipendenze
```

### Commit Atomici
- Un commit = un cambiamento logico
- Se devi usare "e" nel messaggio, probabilmente sono due commit
- Commit frequenti ma significativi

### Branch Naming Convention
```bash
feature/descrizione-breve
bugfix/issue-123-descrizione
hotfix/problema-critico
release/v1.2.0
```

### Workflow Sicuro
1. Pull prima di iniziare a lavorare
2. Crea sempre un branch per le modifiche
3. Testa prima di fare commit
4. Push regolare per backup
5. Code review tramite Pull Request

### .gitignore Essenziale
```gitignore
# Dipendenze
node_modules/
vendor/
venv/

# File di build
dist/
build/
*.exe
*.dll

# IDE
.vscode/
.idea/
*.swp

# OS
.DS_Store
Thumbs.db

# Configurazioni locali
.env
.env.local
config.local.js

# Log
*.log
logs/

# File temporanei
*.tmp
*.cache
```

---

## 10. Risoluzione dei Problemi Comuni

### Annullare Operazioni

```bash
# Annulla modifiche non committate
git checkout -- nomefile.txt

# Rimuovi file dalla staging area
git reset HEAD nomefile.txt

# Modifica l'ultimo commit
git commit --amend -m "Nuovo messaggio"

# Aggiungi file dimenticati all'ultimo commit
git add file-dimenticato.txt
git commit --amend --no-edit
```

### Risolvere Problemi di Push

```bash
# Push rifiutato perché il remote è avanti
git pull --rebase origin main
git push origin main

# Push forzato (ATTENZIONE: sovrascrive il remote)
git push --force-with-lease  # Più sicuro
git push --force             # Da evitare se possibile
```

### Pulire il Repository

```bash
# Rimuovi branch locali già mergiati
git branch --merged | grep -v "\*" | xargs -n 1 git branch -d

# Rimuovi riferimenti a branch remoti eliminati
git remote prune origin

# Pulisci file non tracciati
git clean -n  # Mostra cosa verrebbe eliminato
git clean -f  # Elimina file non tracciati
git clean -fd # Elimina file e directory
```

### Debug e Investigazione

```bash
# Trova chi ha modificato una riga
git blame nomefile.txt

# Trova il commit che ha introdotto un bug
git bisect start
git bisect bad         # Il commit corrente ha il bug
git bisect good abc123 # Questo commit era ok
# Git ti guiderà attraverso i commit
# Testa ogni commit e marca come good/bad
git bisect reset       # Quando finito

# Cerca nel contenuto dei commit
git grep "stringa da cercare"
```

---

## Comandi Rapidi di Riferimento

### I 20 Comandi Più Usati
1. `git init` - Inizializza repository
2. `git clone` - Clona repository
3. `git add` - Aggiungi alla staging area
4. `git commit` - Crea commit
5. `git status` - Mostra stato
6. `git log` - Mostra cronologia
7. `git branch` - Gestisci branch
8. `git checkout` - Cambia branch/ripristina file
9. `git merge` - Unisci branch
10. `git pull` - Scarica e integra modifiche
11. `git push` - Invia modifiche al remote
12. `git diff` - Mostra differenze
13. `git stash` - Salva modifiche temporaneamente
14. `git reset` - Annulla commit (locale)
15. `git revert` - Annulla commit (sicuro)
16. `git fetch` - Scarica senza integrare
17. `git remote` - Gestisci repository remoti
18. `git tag` - Gestisci tag/versioni
19. `git show` - Mostra dettagli commit
20. `git config` - Configura Git

---

## Risorse Aggiuntive

- **Documentazione ufficiale**: [git-scm.com/doc](https://git-scm.com/doc)
- **GitHub Docs**: [docs.github.com](https://docs.github.com)
- **Pro Git Book**: [git-scm.com/book](https://git-scm.com/book)
- **Atlassian Git Tutorial**: [atlassian.com/git](https://www.atlassian.com/git)

## Conclusione

Git è uno strumento potentissimo che richiede pratica per essere padroneggiato. Inizia con i comandi base, adotta gradualmente workflow più complessi e non aver paura di sperimentare (puoi sempre recuperare con `git reflog`!). 

Ricorda: commit frequenti, messaggi chiari e pull request per la collaborazione sono la chiave per un uso efficace di Git.

Buon coding! 🚀