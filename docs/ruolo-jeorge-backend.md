## Ruolo Backend – Jeorge (Laravel)

### 1. Obiettivo del ruolo
Jeorge è responsabile della parte **backend** del progetto, sviluppata con **Laravel**.  
Il suo compito è trasformare i requisiti dell’esercizio in:
- **modelli e tabelle** per sondaggi, opzioni e voti;
- **rotte e controller** che espongono funzionalità di creazione, voto, lista e chiusura;
- una logica robusta che rispetti il vincolo di **un voto per utente per sondaggio**.

Il backend deve fornire un’interfaccia chiara sia:
- al **frontend** (via HTML o API JSON),
- sia alla **base dati** (attraverso le migrazioni e i modelli Eloquent).

---

### 2. Modello dati applicativo

In accordo con il responsabile Database, Jeorge deve implementare in Laravel i seguenti modelli (nomi indicativi):

- **`Poll`**
  - Campi principali:
    - `id`
    - `question` (stringa o testo)
    - `creator_id` (identificativo dell’utente creatore, anche semplificato)
    - `is_open` (booleano: sondaggio aperto o chiuso)
    - `created_at`, `updated_at`
  - Relazioni:
    - `hasMany(Option)`
    - `hasMany(Vote)`

- **`Option`**
  - Campi:
    - `id`
    - `poll_id` (FK verso `polls`)
    - `label` (testo dell’opzione)
  - Relazioni:
    - `belongsTo(Poll)`
    - `hasMany(Vote)`

- **`Vote`**
  - Campi:
    - `id`
    - `poll_id` (FK verso `polls`)
    - `option_id` (FK verso `options`)
    - `user_id` (identificatore univoco dell’utente, definito in modo coerente col resto del progetto)
    - `created_at`
  - Relazioni:
    - `belongsTo(Poll)`
    - `belongsTo(Option)`

L’implementazione in Laravel deve essere coerente con lo schema database concordato con Edward (vedi `docs/ruolo-edward-db.md`).

---

### 3. Rotte e API da implementare

Jeorge deve progettare e realizzare le rotte necessarie per coprire tutti i casi d’uso.  
Le rotte possono essere:
- **HTML** (che restituiscono viste Blade), e/o
- **API JSON** consumate dal frontend tramite fetch/AJAX.

Rotte minime suggerite:

- **Lista sondaggi**
  - Metodo: `GET`
  - URL indicativo: `/polls`
  - Funzione:
    - Restituire l’elenco dei sondaggi, con:
      - `id`, `question`, `is_open`
      - numero totale di voti per ciascun sondaggio (facoltativo ma utile).

- **Creazione sondaggio**
  - Metodo: `POST`
  - URL indicativo: `/polls`
  - Dati in input:
    - `question`: testo della domanda;
    - `options`: array di 3–5 stringhe.
  - Comportamento:
    - Validare che ci siano tra 3 e 5 opzioni non vuote;
    - creare il record `Poll`;
    - creare i record associati in `options`.

- **Dettaglio sondaggio**
  - Metodo: `GET`
  - URL indicativo: `/polls/{id}`
  - Funzione:
    - Restituire:
      - dati del sondaggio;
      - elenco opzioni;
      - conteggio voti per ogni opzione.

- **Voto su un sondaggio**
  - Metodo: `POST`
  - URL indicativo: `/polls/{id}/vote`
  - Dati in input:
    - `option_id` (opzione scelta);
    - identificazione dell’utente (`user_id` o informazione derivata).
  - Comportamento:
    - Verificare che il sondaggio sia **aperto**;
    - verificare che l’opzione appartenga a quel sondaggio;
    - controllare se l’utente ha già votato (vedi sezione 4);
    - in caso positivo, bloccare con errore;
    - altrimenti, registrare il voto.

- **Chiusura sondaggio**
  - Metodo: `POST` o `PATCH`
  - URL indicativo: `/polls/{id}/close`
  - Comportamento:
    - Verificare che chi fa la richiesta sia il **creatore** del sondaggio;
    - impostare `is_open = false`;
    - non cancellare i voti, ma solo impedire nuovi voti.

---

### 4. Logica del voto e vincolo “un voto per utente”

Questa è la parte più delicata del backend. Jeorge deve garantire che:
- per ogni coppia (`poll_id`, `user_id`) esista **al massimo un voto**.

Strategia consigliata:

- **A livello applicativo (Laravel)**
  - Prima di inserire un nuovo voto:
    - effettuare una query tipo:
      - `Vote::where('poll_id', $pollId)->where('user_id', $userId)->exists()`
    - se esiste già un voto:
      - restituire un errore (es. status 400 o 422) con messaggio chiaro (“Hai già votato questo sondaggio”).

- **A livello di database**
  - Lavorare con Edward per definire un **vincolo univoco** sulla tabella `votes`:
    - chiave univoca su (`poll_id`, `user_id`);
  - Questo impedisce doppi voti anche in caso di richieste contemporanee.

Jeorge deve gestire correttamente gli errori che arrivano dalla logica applicativa o dal database, traducendoli in risposte comprensibili per il frontend.

---

### 5. Contratti di interfaccia col frontend

Per aiutare Jefferson, Jeorge deve documentare **come** si usano le rotte.  
Ad esempio, per ogni endpoint API dovrebbe fornire:

- **Esempio request** (JSON):

```json
POST /polls
{
  "question": "Qual è il tuo linguaggio preferito?",
  "options": ["PHP", "JavaScript", "Python"]
}
```

- **Esempio response** (JSON):

```json
{
  "id": 1,
  "question": "Qual è il tuo linguaggio preferito?",
  "is_open": true,
  "options": [
    { "id": 10, "label": "PHP" },
    { "id": 11, "label": "JavaScript" },
    { "id": 12, "label": "Python" }
  ]
}
```

Per il voto:

```json
POST /polls/1/vote
{
  "option_id": 11
}
```

Esempio response in caso di successo:

```json
{
  "message": "Voto registrato correttamente"
}
```

Esempio response in caso di errore (utente ha già votato):

```json
{
  "error": "Hai già votato questo sondaggio"
}
```

Analogamente, Jeorge può definire le risposte per:
- lista sondaggi (`GET /polls`);
- dettaglio (`GET /polls/{id}`);
- chiusura (`POST/PATCH /polls/{id}/close`).

---

### 6. Task concreti per Jeorge

1. **Configurare i modelli Laravel**
   - Creare i modelli `Poll`, `Option`, `Vote` con le relazioni corrette.
   - Verificare che le migrazioni create da Edward siano correttamente collegate.

2. **Implementare le rotte principali**
   - Definire nel file `routes/web.php` o `routes/api.php` (a seconda dell’approccio) le rotte elencate in sezione 3.
   - Collegarle a controller dedicati (es. `PollController`, `VoteController`).

3. **Sviluppare la logica di voto**
   - Implementare il controllo “un voto per utente”.
   - Restituire messaggi chiari in caso di errore.

4. **Preparare i dati per i risultati**
   - Nel dettaglio sondaggio, calcolare il numero di voti per ogni opzione.
   - Restituire questi conteggi in forma facilmente consumabile dal frontend.

5. **Testare manualmente le API**
   - Usare strumenti come Postman o curl (o anche rotte HTML temporanee) per verificare:
     - creazione sondaggio;
     - voto;
     - blocco doppio voto;
     - chiusura sondaggio.

---

### 7. Output atteso dal ruolo Backend

A fine progetto, Jeorge dovrebbe consegnare:
- un backend Laravel che:
  - gestisce correttamente sondaggi, opzioni e voti;
  - applica i vincoli richiesti (un voto per utente, impossibilità di votare sondaggi chiusi);
  - espone API/rotte chiare e documentate;
- un set minimo di test manuali o note che descrivono come provare le funzionalità;
- una buona integrazione con:
  - lo **schema database** di Edward;
  - il **frontend** di Jefferson, grazie a contratti API ben definiti.

