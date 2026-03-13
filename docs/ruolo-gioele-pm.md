## Ruolo di Project Manager – Gioele

### 1. Obiettivo del ruolo
Il Project Manager (PM) ha il compito di **guidare il progetto Sondaggi**, coordinare il lavoro dei compagni e assicurarsi che il risultato finale rispetti:
- i **requisiti dell’esercizio** (creazione sondaggio, voto, risultati, lista, chiusura);
- le **regole di collaborazione** (Git, branch, Pull Request);
- la **qualità minima** del codice e dell’esperienza utente.

Il PM **non deve scrivere per forza tutto il codice**, ma deve:
- capire come funziona il sistema;
- saper leggere i vari documenti e collegarli;
- aiutare il team a prendere decisioni e risolvere problemi.

---

### 2. Responsabilità principali

- **Organizzazione del lavoro**
  - Tradurre i requisiti dell’esercizio in una lista di funzionalità concrete.
  - Distribuire le funzionalità tra Backend, Frontend e Database.
  - Tenere aggiornato lo stato di avanzamento (cosa è fatto, cosa manca).

- **Gestione tecnica del repository**
  - Definire e spiegare alla squadra la strategia Git:
    - branch protetti (`master`, `develop`);
    - branch di feature (es. `jeorge/backend-polls`, `jeff/frontend-ui`, `edward/db-schema`).
  - Controllare che nessuno lavori direttamente su `master` o `develop`.
  - Assicurarsi che ogni modifica importante passi tramite **Pull Request**.

- **Qualità e requisiti**
  - Verificare che ogni parte del sistema:
    - rispetti la logica “un voto per utente”;
    - permetta di creare sondaggi con 3–5 opzioni;
    - mostri i risultati in modo chiaro;
    - permetta di chiudere un sondaggio solo al creatore.
  - Coordinare correzioni e miglioramenti quando qualcosa non funziona come richiesto.

- **Comunicazione**
  - Organizzare brevi confronti con il team per allineare lo stato dei lavori.
  - Fare da “ponte” tra Backend, Frontend e Database quando servono chiarimenti (es. formato API, struttura tabelle).

---

### 3. Compiti operativi per il progetto

#### 3.1 Preparazione iniziale
- Leggere attentamente:
  - la documentazione generale del progetto in `docs/progetto-sondaggi.md`;
  - questo documento di ruolo;
  - i documenti dei compagni per capire chi farà cosa.
- Concordare con il team lo **stack definitivo**:
  - uso di Laravel per il backend;
  - uso di Blade/Tailwind o di una UI con Vite per il frontend;
  - tipo di database usato in sviluppo (es. SQLite).

#### 3.2 Definizione delle attività
- Creare un elenco di **attività principali**, ad esempio:
  - “Modello dati sondaggi/opzioni/voti”;
  - “API per creare sondaggio”;
  - “API per votare con controllo un voto per utente”;
  - “Pagina lista sondaggi”;
  - “Pagina dettaglio sondaggio con risultati”;
  - “Funzione di chiusura sondaggio”.
- Assegnare ogni attività a:
  - Backend (Jeorge),
  - Frontend (Jefferson),
  - Database (Edward),
  a seconda delle competenze e del ruolo definito.

#### 3.3 Monitoraggio del progresso
- Chiedere periodicamente a ciascun membro:
  - cosa è stato fatto;
  - cosa rimane da fare;
  - se ci sono blocchi (tecnici o di comprensione).
- Prendere decisioni su:
  - eventuale riduzione o semplificazione di alcune parti;
  - priorità (es. prima far funzionare la logica di voto, poi rifinire la grafica).

---

### 4. Flusso Git e regole di collaborazione

#### 4.1 Strategia dei branch
- Tenere **intatti** i branch:
  - `master`: versione finale o molto stabile;
  - `develop`: versione di integrazione dove confluiscono le feature approvate.
- Per ogni funzionalità o gruppo di attività creare un branch del tipo:
  - `nome/feature-descrizione`
    - esempi:
      - `jeorge/backend-polls-api`
      - `jeff/frontend-lista-sondaggi`
      - `edward/db-schema-voti`

#### 4.2 Regole per i commit
- Richiedere al team di:
  - fare **commit frequenti**, ma significativi (non tutto in un unico commit enorme);
  - usare messaggi chiari, ad esempio:
    - `feat: aggiunta API creazione sondaggio`
    - `fix: impedito doppio voto per utente`
    - `style: migliorata UI risultati sondaggio`

#### 4.3 Pull Request e merge
- Ogni volta che una funzionalità è pronta:
  - lo sviluppatore apre una **Pull Request** dal proprio branch verso `develop`;
  - la PR deve avere:
    - titolo chiaro (es. “Backend: API creazione e lista sondaggi”);
    - descrizione con elenco breve dei cambiamenti.
- Prima del merge, il PM:
  - legge la descrizione;
  - **fa un minimo di test manuale** (se possibile);
  - verifica che non ci siano conflitti evidenti con altre parti.
- Solo dopo questi controlli, il PM conferma il merge su `develop`.

---

### 5. Checklist di accettazione per le funzionalità

Per ogni PR o funzionalità, il PM dovrebbe verificare:

- **Requisiti funzionali**
  - La funzionalità soddisfa parte dell’esercizio (creazione, voto, risultati, lista, chiusura)?
  - Il comportamento è coerente con la documentazione in `docs/progetto-sondaggi.md`?

- **Logica di business**
  - La regola “un voto per utente” è rispettata (sia lato backend che lato database)?
  - I sondaggi chiusi non accettano nuovi voti?

- **Interfaccia utente**
  - Le pagine interessate sono comprensibili e usabili?
  - Ci sono messaggi di errore/feedback ragionevoli (es. “Hai già votato”, “Sondaggio chiuso”)?

- **Coerenza tecnica**
  - Le nuove rotte/API sono documentate (almeno a livello base) e usabili dal frontend?
  - I nomi delle tabelle, campi e modelli sono coerenti fra backend e database?

- **Stato del repository**
  - Il branch si integra senza conflitti gravi in `develop`?
  - Non ci sono file inutili o temporanei inclusi (ad es. file locali, cache, ecc.)?

Se uno o più punti non sono rispettati, il PM segnala le modifiche richieste prima di approvare la PR.

---

### 6. Collaborazione con gli altri ruoli

- **Con Backend (Jeorge)**
  - Chiarire quali endpoint servono per coprire le schermate pianificate.
  - Concordare i formati JSON di richiesta e risposta per facilitare il lavoro del frontend.

- **Con Frontend (Jefferson)**
  - Verificare che tutte le funzionalità richieste siano rappresentate nell’interfaccia (creazione, voto, risultati, lista, chiusura).
  - Dare feedback sulla chiarezza dell’UI e sulle informazioni mostrate all’utente.

- **Con Database (Edward)**
  - Valutare insieme che lo schema tabelle soddisfi tutti i requisiti (in particolare la limitazione di un voto per utente).
  - Assicurarsi che le migrazioni siano coerenti con quello che serve al backend.

---

### 7. Output atteso dal PM

A fine progetto, il lavoro del Project Manager dovrebbe aver prodotto:
- una **documentazione chiara** del progetto e dei ruoli (insieme a questo file);
- un **flusso Git ordinato**, con branch e PR che raccontano lo sviluppo in modo leggibile;
- un’applicazione complessiva in cui:
  - backend, frontend e database sono coerenti;
  - i requisiti dell’esercizio sono rispettati;
  - il lavoro di squadra è riconoscibile e ben organizzato.

