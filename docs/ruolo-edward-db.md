## Ruolo Database – Edward

### 1. Obiettivo del ruolo
Edward è responsabile della **progettazione e gestione del database** del progetto Sondaggi.  
Il suo obiettivo è definire uno **schema relazionale** che:
- rappresenti correttamente sondaggi, opzioni e voti;
- garantisca il vincolo di **un solo voto per utente per ciascun sondaggio**;
- sia coerente con i modelli Eloquent implementati da Jeorge (backend Laravel);
- supporti in modo efficiente le operazioni richieste (lista, voto, risultati, chiusura).

---

### 2. Schema concettuale ed ER

Edward deve partire da una visione concettuale del dominio, identificando le seguenti entità:

- **Sondaggio (`Poll`)**
  - Identifica un sondaggio creato da un utente.
  - Attributi principali:
    - `id`
    - `question` (testo della domanda)
    - `creator_id` (identificativo dell’utente creatore, definito in accordo con il resto del team)
    - `is_open` (booleano: indica se il sondaggio è votabile)
    - `created_at`, `updated_at`

- **Opzione (`Option`)**
  - Rappresenta una singola opzione di risposta di un sondaggio.
  - Attributi:
    - `id`
    - `poll_id` (chiave esterna verso `Poll`)
    - `label` (testo dell’opzione)

- **Voto (`Vote`)**
  - Rappresenta il voto espresso da un utente su un sondaggio.
  - Attributi:
    - `id`
    - `poll_id` (chiave esterna verso `Poll`)
    - `option_id` (chiave esterna verso `Option`)
    - `user_id` (identificatore dell’utente che vota)
    - `created_at`

Relazioni principali:
- Un **Poll** ha molte **Option** (1–N).
- Un **Poll** ha molti **Vote** (1–N).
- Una **Option** ha molti **Vote** (1–N).

---

### 3. Vincoli e chiavi

Edward deve progettare lo schema con particolare attenzione ai vincoli, per garantire la **coerenza dei dati**.

- **Chiavi primarie**
  - Ogni tabella (`polls`, `options`, `votes`) deve avere una chiave primaria (tipicamente un campo `id` auto–incrementale).

- **Chiavi esterne**
  - `options.poll_id` → `polls.id`
  - `votes.poll_id` → `polls.id`
  - `votes.option_id` → `options.id`

- **Vincolo “un voto per utente per sondaggio”**
  - Nella tabella `votes` deve essere presente un **vincolo univoco** sulla coppia:
    - (`poll_id`, `user_id`)
  - Questo impedisce che lo stesso utente inserisca due record di voto per lo stesso sondaggio, anche in caso di richieste concorrenti.

- **Stato del sondaggio**
  - Il campo `is_open` in `polls` permette di:
    - sapere se un sondaggio è ancora votabile;
    - facilitare query lato backend per filtrare solo i sondaggi aperti.

Edward deve confrontarsi con Jeorge per allineare nomi e tipi dei campi a quelli utilizzati nei modelli Laravel.

---

### 4. Migrazioni Laravel e dati di test

L’implementazione tecnica dello schema avviene tramite le **migrazioni** in Laravel (directory `database/migrations`).

#### 4.1 Piano per le migrazioni

Ordine consigliato:
1. Migrazione per la tabella `polls`
   - Creare i campi:
     - `id`
     - `question`
     - `creator_id`
     - `is_open` (default `true`)
     - `timestamps` (Laravel: `created_at`, `updated_at`)
2. Migrazione per la tabella `options`
   - Campi:
     - `id`
     - `poll_id` (FK verso `polls`)
     - `label`
3. Migrazione per la tabella `votes`
   - Campi:
     - `id`
     - `poll_id` (FK verso `polls`)
     - `option_id` (FK verso `options`)
     - `user_id`
     - `created_at`
   - Aggiungere:
     - vincolo di chiave esterna;
     - **indice univoco** su (`poll_id`, `user_id`).

Ogni migrazione deve prevedere anche il metodo `down()` per permettere il **rollback** in caso di errori.

#### 4.2 Seeder per dati di esempio

Edward può creare uno o più **seeder** (in `database/seeders`) per:
- popolare il database con:
  - 1–2 sondaggi di esempio;
  - 3–5 opzioni per ogni sondaggio;
  - alcuni voti finti (coerenti col vincolo un voto per utente).
- facilitare i test:
  - della lista sondaggi;
  - della visualizzazione dei risultati.

Questo aiuta soprattutto Jefferson (frontend) e il PM a verificare rapidamente il funzionamento dell’applicazione.

---

### 5. Collaborazione con il Backend

Edward deve lavorare a stretto contatto con Jeorge per:

- **Allineare nomi e convenzioni**
  - Usare nomi di tabelle e campi coerenti con Eloquent:
    - tabelle al plurale (`polls`, `options`, `votes`);
    - chiavi esterne nel formato standard (`poll_id`, `option_id`, `user_id`).

- **Supportare i casi d’uso del backend**
  - Verificare che le query di Jeorge siano efficienti:
    - ad esempio, creare indici dove necessario (su `poll_id`, `user_id`, ecc.).
  - Garantire che lo schema supporti:
    - il conteggio voti per opzione;
    - il filtraggio dei sondaggi aperti/chiusi.

- **Gestione della chiusura dei sondaggi**
  - La chiusura di un sondaggio **non elimina** i voti:
    - serve solo a segnare `is_open = false`.
  - L’integrità referenziale va mantenuta:
    - se in futuro si decidesse di cancellare un sondaggio, valutare cosa succede a opzioni e voti (es. cancellazione in cascata o altro comportamento definito).

---

### 6. Task concreti per Edward

1. **Definizione schema ER**
   - Disegnare (anche su carta) lo schema concettuale con entità e relazioni.
   - Condividerlo con il team per conferma.

2. **Creazione migrazioni**
   - Scrivere le migrazioni per `polls`, `options`, `votes` seguendo il piano descritto.
   - Inserire le chiavi esterne e il vincolo univoco su (`poll_id`, `user_id`).

3. **Configurazione indici**
   - Aggiungere indici dove utile (es. su `poll_id` in `options` e `votes`, su `user_id` in `votes`).

4. **Creazione seeder di esempio**
   - Preparare uno o più seeder con dati di esempio per sondaggi, opzioni e voti.
   - Testare che `php artisan migrate --seed` funzioni correttamente.

5. **Verifica con il backend**
   - Collaborare con Jeorge per correggere eventuali disallineamenti tra schema e codice.
   - Adattare lo schema se emergono nuove esigenze (es. tracciamento di ulteriori informazioni).

---

### 7. Output atteso dal ruolo Database

A fine progetto, Edward dovrebbe consegnare:
- uno **schema database stabile**, coerente e ben documentato;
- un set di **migrazioni Laravel** funzionanti;
- eventuali **seeder** che permettono di avere dati realistici per i test;
- una collaborazione efficace con il backend, tale che:
  - il vincolo “un voto per utente per ciascun sondaggio” sia realmente garantito;
  - tutte le funzionalità richieste (creazione, voto, risultati, lista, chiusura) siano supportate dal punto di vista dei dati.

