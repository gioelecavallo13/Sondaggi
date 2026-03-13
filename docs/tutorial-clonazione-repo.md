## Tutorial: clonare il repository del progetto “Sondaggi” sul proprio PC di casa

Questo tutorial spiega, passo passo, come **clonare il repository Git del progetto “Sondaggi”** sul tuo computer di casa.

L’obiettivo è che **ogni membro del team** (PM, backend, frontend, DB) riesca ad avere in locale la stessa copia del codice per poter lavorare seguendo le proprie istruzioni di ruolo.

---

### 1. Prerequisiti

Prima di iniziare, verifica di avere:

- **Git installato sul tuo computer**
  - Per controllare, apri il terminale (vedi sezione successiva) e digita:

    ```bash
    git --version
    ```

  - Se il comando restituisce qualcosa tipo `git version 2.x.x`, Git è installato.
  - Se il comando non viene riconosciuto:
    - **Windows**: scarica l’installer da `https://git-scm.com/download/win` e segui l’installazione guidata.
    - **macOS**: puoi installare Git tramite Xcode Command Line Tools (ti verrà chiesto automaticamente al primo uso) oppure da `https://git-scm.com/download/mac`.
    - **Linux**: usa il gestore pacchetti della tua distribuzione (ad es. `sudo apt install git` su Ubuntu).

- **Accesso al provider Git (es. GitHub, GitLab, Bitbucket)**
  - Devi essere **aggiunto al repository** del progetto “Sondaggi” (di solito tramite invito dal Project Manager o dall’insegnante).

- **URL del repository**
  - È lo stesso per tutti i membri del team.
  - Lo troverai nella pagina del repository, in genere con due opzioni:
    - URL **HTTPS**
    - URL **SSH**

Tieni a portata di mano l’URL che ti è stato fornito (oppure copialo dalla pagina del repository).

---

### 2. Aprire il terminale / prompt dei comandi

Per l’operazione di clonazione useremo il terminale:

- **Windows**
  - Puoi usare **Git Bash** (installato insieme a Git) oppure **Prompt dei comandi** o **PowerShell**.
  - Cerca “Git Bash” o “Prompt dei comandi” dal menu Start.

- **macOS**
  - Apri l’app **Terminale** da `Applicazioni → Utility → Terminale`, oppure cerca “Terminale” con Spotlight.

- **Linux**
  - Apri il tuo emulatore di terminale preferito (ad es. “Terminale”, “Konsole”, “GNOME Terminal”, ecc.).

Una volta aperto il terminale, decidi **in quale cartella** vuoi salvare il progetto.
Ad esempio, puoi usare una cartella `Sviluppo` nella tua home.

Esempio (facoltativo):

```bash
mkdir -p ~/Sviluppo
cd ~/Sviluppo
```

Su Windows con Git Bash puoi fare qualcosa di simile:

```bash
mkdir -p ~/Sviluppo
cd ~/Sviluppo
```

---

### 3. Ottenere l’URL del repository

Vai nella pagina web del repository del progetto “Sondaggi”.

In genere troverai un pulsante tipo **“Code”** o **“Clone”**:

- Seleziona la scheda **HTTPS** per ottenere l’URL HTTPS (esempio fittizio):

  ```text
  https://github.com/nome-organizzazione/sondaggi.git
  ```

- Oppure seleziona la scheda **SSH** per ottenere l’URL SSH (esempio fittizio):

  ```text
  git@github.com:nome-organizzazione/sondaggi.git
  ```

Copiati l’URL che userai nel comando `git clone`.

---

### 4. Clonare il repository via HTTPS

Questo è il metodo più semplice se **non hai ancora configurato le chiavi SSH**.

1. **Posizionati nella cartella di lavoro** (ad esempio `~/Sviluppo`, come visto sopra):

   ```bash
   cd ~/Sviluppo
   ```

2. **Esegui il comando `git clone` con l’URL HTTPS**:

   ```bash
   git clone https://github.com/nome-organizzazione/sondaggi.git
   ```

   (Sostituisci l’URL con quello reale del tuo repository.)

3. **Autenticazione (se richiesta)**:
   - Il sistema potrebbe chiederti **username** e **password** del tuo account (GitHub/GitLab, ecc.).
   - Alcuni provider richiedono l’uso di un **Personal Access Token (PAT)** al posto della password.
     - In questo caso dovrai:
       - Creare il token dal tuo profilo sul sito (seguendo la guida del provider).
       - Copiare il token e incollarlo nel prompt quando ti viene richiesta la password.

4. **Attendi che la clonazione finisca**:
   - Vedrai scorrere una serie di messaggi tipo:
     - `Cloning into 'sondaggi'...`
     - `remote: Counting objects...`
     - `Receiving objects: 100% ...`

Al termine, nella cartella di lavoro troverai una **nuova cartella** (ad es. `sondaggi`) con dentro tutti i file del progetto.

---

### 5. Clonare il repository via SSH

La clonazione via SSH è comoda se **lavori spesso sul repository**, perché ti evita di inserire username/password o token ad ogni operazione.

#### 5.1 Verificare se hai già una chiave SSH

Nel terminale, digita:

```bash
ls ~/.ssh
```

Se vedi file come `id_rsa`, `id_rsa.pub`, `id_ed25519`, `id_ed25519.pub` o simili, probabilmente hai già una coppia di chiavi SSH.

#### 5.2 Generare una nuova chiave SSH (se necessario)

Se non hai una chiave (o vuoi crearne una dedicata), puoi generarla con:

```bash
ssh-keygen -t ed25519 -C "tuo_email@example.com"
```

Se il tuo sistema non supporta `ed25519`, puoi usare:

```bash
ssh-keygen -t rsa -b 4096 -C "tuo_email@example.com"
```

Premi **Invio** quando ti chiede dove salvare il file (per accettare il percorso di default) e, se vuoi, imposta una **passphrase** per maggiore sicurezza.

#### 5.3 Copiare la chiave pubblica nel provider (es. GitHub)

1. Visualizza la chiave pubblica:

   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```

   (Oppure `id_rsa.pub`, se hai creato una chiave RSA.)

2. Copia **tutta la riga** che inizia con `ssh-ed25519` o `ssh-rsa`.

3. Vai sul sito del provider (es. GitHub):
   - Apri le **Impostazioni** del tuo account.
   - Cerca la sezione **SSH and GPG keys** (o equivalente).
   - Aggiungi una **nuova chiave SSH**, incolla il contenuto della tua chiave pubblica e salva.

#### 5.4 Testare la connessione SSH

Esegui:

```bash
ssh -T git@github.com
```

(Sostituisci `github.com` con il dominio effettivo del tuo provider se diverso.)

Potresti vedere un messaggio di questo tipo la prima volta:

```text
The authenticity of host 'github.com (IP ADDRESS)' can't be established.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Scrivi `yes` e premi Invio.

Se tutto è configurato correttamente, dovresti ottenere un messaggio di benvenuto dal provider.

#### 5.5 Clonare il repository con l’URL SSH

1. Torna nella **cartella di lavoro** (es. `~/Sviluppo`):

   ```bash
   cd ~/Sviluppo
   ```

2. Usa il comando:

   ```bash
   git clone git@github.com:nome-organizzazione/sondaggi.git
   ```

   (Sostituisci l’URL con quello reale in formato SSH che hai copiato dalla pagina del repository.)

3. Attendi la fine della clonazione.

Ora avrai la cartella del progetto clonata via SSH e potrai usare `git pull`, `git push`, ecc. senza dover reinserire le credenziali ogni volta.

---

### 6. Verificare che la clonazione sia avvenuta correttamente

1. **Controllare che la cartella esista**
   - Nella cartella di lavoro (ad es. `~/Sviluppo`) dovresti vedere una nuova cartella, ad esempio `sondaggi`.

2. **Entrare nella cartella del progetto**

   ```bash
   cd sondaggi
   ```

3. **Verificare lo stato di Git**

   ```bash
   git status
   ```

   - Dovresti vedere un messaggio tipo:

     ```text
     On branch main
     nothing to commit, working tree clean
     ```

   - Questo indica che la cartella locale è sincronizzata con il repository remoto.

4. **Elenco dei file**
   - Puoi controllare i file presenti:

     ```bash
     ls
     ```

   - Dovresti vedere, tra gli altri, file come `README.md`, la cartella `docs`, la cartella del codice backend/frontend, ecc.

---

### 7. Passi successivi per i diversi ruoli

Una volta clonata correttamente la repository e posizionato il terminale nella cartella del progetto (ad es. `cd sondaggi`), ogni membro del team può seguire le **istruzioni specifiche del proprio ruolo**:

- **Project Manager (PM)**: vedi il documento `[docs/ruolo-gioele-pm.md](./ruolo-gioele-pm.md)`.
- **Backend**: vedi il documento `[docs/ruolo-jeorge-backend.md](./ruolo-jeorge-backend.md)`.
- **Frontend**: vedi il documento `[docs/ruolo-jefferson-frontend.md](./ruolo-jefferson-frontend.md)`.
- **Database**: vedi il documento `[docs/ruolo-edward-db.md](./ruolo-edward-db.md)`.

Inoltre, puoi consultare:

- La documentazione generale del progetto in `[docs/progetto-sondaggi.md](./progetto-sondaggi.md)`.
- Gli eventuali file di stato personali (ad es. `docs/stato-nome.txt`) per tenere traccia dei tuoi avanzamenti.

Con questi passaggi, **tutti i membri del team** saranno in grado di clonare il repository sul proprio PC di casa e iniziare a lavorare sul progetto “Sondaggi” in modo coerente e coordinato.

