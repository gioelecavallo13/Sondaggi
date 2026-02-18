## Informazioni sul progetto
**Data di assegnazione:** 18/02/2026  
**Data di consegna:** Aprile 2026

### Team e Ruoli
1. **Gioele Cavallo:** Project Manager, gestione infrastrutture, problem solver  
2. **Jeorge Mantala:** Sviluppo backend  
3. **Jefferson Martinez:** Sviluppo frontend  
4. **Edward Amante:** Creazione e gestione database

---

## 📌 Descrizione del progetto
Il progetto **Sondaggi** consiste nello sviluppo di un sistema che permette a un utente di **creare sondaggi online** e ad altri utenti di **partecipare votando**.

Ogni sondaggio è composto da una domanda e da un numero limitato di opzioni di risposta.  
Il sistema garantisce che ogni utente possa **votare una sola volta** per ciascun sondaggio e consente di **visualizzare i risultati aggiornati in tempo reale**.

Il progetto è sviluppato in **collaborazione tra più persone**, utilizzando **Git e GitHub** per la gestione del codice e seguendo una strategia di branch che protegge il codice stabile e facilita il lavoro di squadra.

---

## 🧑‍💻 Lavorare al progetto con Visual Studio Code (modalità grafica)

### 🔽 Clonare il repository
1. Aprire **Visual Studio Code**
2. Premere **Ctrl + Shift + P** per aprire la *Command Palette*
3. Scrivere **Git: Clone** e selezionarlo
4. Incollare l’URL del repository GitHub: `https://github.com/gioelecavallo13/Sondaggi.git`
5. Scegliere la cartella dove salvare il progetto
6. Cliccare su **Open** quando VS Code chiede di aprire il progetto

---

### 🌿 Creare e lavorare sul proprio branch
1. In basso a sinistra, cliccare sul nome del branch (es. `master` o `develop`)
2. Selezionare **Create new branch**
3. Inserire il nome del branch (es. `nome_collaboratore/feature_da_implementare`)
4. Premere **Invio**

👉 Tutto il lavoro verrà ora fatto su questo branch.

---

### 💾 Salvare le modifiche e creare un commit
1. Aprire il pannello **Explorer** (icona file a sinistra)
2. Aprire il menu a tendina **Source Control: changes**
2. Visualizzare la lista dei file modificati
3. Cliccare sul **+** accanto ai file per aggiungerli al commit (salvataggio)
4. Scrivere un messaggio chiaro nel campo del commit
5. Premere **✓ Commit**

---

### 🚀 Pubblicare il branch su GitHub
1. Dopo il commit, VS Code mostrerà il pulsante **Publish Branch**
2. Cliccare su **Publish Branch**
3. Attendere il completamento del push

---

### 🔁 Creare una richiesta di merge (Pull Request)
1. Aprire il repository su **GitHub**
2. Comparirà un messaggio con il nuovo branch
3. Cliccare su **Compare & pull request**
4. Impostare:
   - **Branch di destinazione:** `develop`
   - **Branch di origine:** il proprio branch
5. Inserire titolo e descrizione della Pull Request
6. Cliccare su **Create pull request**

---

### ⚠️ Regole importanti
- Non lavorare mai direttamente su `master` o `develop`  
- Ogni funzionalità deve avere il proprio branch  
- Il merge (unione) avviene solo tramite Pull Request, e solo il PM può accettare il merge, dopo aver verificato il corretto funzionamento della feature implementata.

---

### Materiale disponibile

1. branch **fitlife/views/example**, contiene il codice per realizzare il frontend del progetto della palestra di Gioele. Questo può essere utile a Jeff per realizzare la struttura del progetto **sondaggi**
