## Ruolo Frontend – Jefferson

### 1. Obiettivo del ruolo
Jefferson è responsabile della **parte visiva e interattiva** del progetto Sondaggi.  
Il suo compito è progettare e sviluppare le pagine che permettono agli utenti di:
- vedere la **lista dei sondaggi disponibili**;
- **creare** un nuovo sondaggio (domanda + 3–5 opzioni);
- **votare** un sondaggio (un solo voto per utente);
- vedere i **risultati aggiornati** (numero voti per opzione);
- capire se un sondaggio è **aperto o chiuso**.

Il frontend deve essere:
- chiaro e semplice da usare;
- ben collegato alle API del backend sviluppate da Jeorge;
- coerente con la documentazione generale in `docs/progetto-sondaggi.md`.

---

### 2. Pagine principali da realizzare

#### 2.1 Home / Lista sondaggi
- Mostra tutti i sondaggi presenti (almeno quelli attivi, opzionalmente anche chiusi).
- Per ogni sondaggio visualizzare:
  - **domanda**;
  - **stato** (aperto/chiuso);
  - eventualmente: numero totale di voti;
  - un **link/pulsante** per accedere al dettaglio.
- Azioni:
  - pulsante per andare alla **pagina di creazione sondaggio**.

#### 2.2 Pagina dettaglio sondaggio
- Mostra:
  - la domanda del sondaggio;
  - le 3–5 opzioni disponibili (come radio button o pulsanti);
  - lo **stato** del sondaggio (aperto/chiuso);
  - la sezione **risultati**, con il conteggio voti per ogni opzione.
- Azioni:
  - se il sondaggio è **aperto**:
    - permettere all’utente di selezionare **una sola opzione**;
    - inviare il voto al backend;
    - bloccare un secondo voto (anche lato interfaccia).
  - se il sondaggio è **chiuso**:
    - disabilitare il form di voto;
    - mostrare solo i risultati.

#### 2.3 Pagina creazione sondaggio
- Form con i campi:
  - input testo per la **domanda**;
  - 3–5 campi testo per le **opzioni** (possibilità di aggiungere/rimuovere fino a massimo 5).
- Validazioni lato client:
  - la domanda non può essere vuota;
  - devono esistere **almeno 3 opzioni** non vuote;
  - non più di 5 opzioni.
- Invio:
  - al submit, inviare una richiesta al backend (es. `POST /polls`);
  - gestire la risposta:
    - in caso di successo: mostrare un messaggio e/o reindirizzare alla lista o al dettaglio;
    - in caso di errore: visualizzare un messaggio adeguato.

---

### 3. Flussi di interazione

#### 3.1 Flusso “crea sondaggio”
1. L’utente apre la **pagina creazione sondaggio** dalla home.
2. Compila la domanda e le opzioni.
3. Clicca su “Crea sondaggio”.
4. Il frontend:
   - valida i dati;
   - invia una richiesta al backend (`POST /polls`) con un JSON simile a:

```json
{
  "question": "Domanda di esempio",
  "options": ["Opzione 1", "Opzione 2", "Opzione 3"]
}
```

5. Se il backend risponde con successo:
   - mostra un messaggio di conferma;
   - eventualmente, reindirizza alla **lista sondaggi** o al **dettaglio** del sondaggio appena creato.
6. Se ci sono errori (validazione server, problemi di rete, ecc.):
   - mostra messaggi chiari (es. “Inserisci almeno 3 opzioni”, “Errore del server, riprova più tardi”).

#### 3.2 Flusso “vota sondaggio”
1. L’utente apre la **pagina dettaglio sondaggio** dalla lista.
2. Se il sondaggio è aperto:
   - seleziona una delle opzioni;
   - clicca su “Vota”.
3. Il frontend invia una richiesta al backend (`POST /polls/{id}/vote`) con qualcosa come:

```json
{
  "option_id": 11
}
```

4. Se il voto viene accettato:
   - disabilita ulteriori voti dall’interfaccia (es. disabilitando il pulsante o le radio);
   - mostra un messaggio (“Voto registrato correttamente”);
   - aggiorna i risultati (vedi sezione successiva).
5. Se il backend restituisce un errore (es. “Hai già votato questo sondaggio” o “Sondaggio chiuso”):
   - mostra il messaggio di errore nel layout.

---

### 4. Aggiornamento dei risultati in tempo “reale”

Per “risultati in tempo reale” si intende che, mentre gli utenti votano, i conteggi sullo schermo si aggiornano **automaticamente**, senza ricaricare manualmente tutta la pagina.

Nel contesto di questo esercizio, la soluzione consigliata è:

- **Polling periodico semplice**
  - Sul dettaglio sondaggio, il frontend:
    - chiama periodicamente il backend (es. ogni 3–5 secondi) con `GET /polls/{id}`;
    - legge i nuovi conteggi dei voti per opzione;
    - aggiorna la sezione risultati (es. numeri o barre).
  - Quando il sondaggio risulta chiuso (`is_open = false`):
    - può interrompere il polling oppure continuare solo come visualizzazione.

Estensione opzionale (non obbligatoria):
- **Uso di WebSocket / eventi broadcast**
  - Ricevere notifiche in tempo reale quando viene registrato un nuovo voto.
  - Aggiornare la UI istantaneamente senza polling.

Per l’esercizio di base è sufficiente implementare il **polling**.

---

### 5. Linee guida UI/UX

- **Chiarezza**
  - Evidenziare sempre:
    - la domanda del sondaggio;
    - lo stato (aperto/chiuso);
    - il numero di voti per opzione.
  - Usare etichette chiare per i pulsanti:
    - “Crea sondaggio”,
    - “Vota”,
    - “Torna alla lista”.

- **Feedback immediato**
  - Dopo il voto:
    - mostrare un messaggio di conferma o di errore;
    - rendere chiaro che l’utente ha già votato (ad esempio, disabilitando il modulo o aggiungendo una scritta).
  - Dopo la creazione di un sondaggio:
    - indicare chiaramente cosa è successo (“Sondaggio creato con successo”).

- **Uso di Tailwind / CSS moderni**
  - Organizzare il layout con una struttura pulita (card, spacing, typography).
  - Mantenere uno stile uniforme per pulsanti, input e titoli.

- **Responsività di base**
  - La UI dovrebbe rimanere leggibile su:
    - schermi desktop;
    - tablet o schermi più piccoli (almeno con layout a colonna).

---

### 6. Integrazione con il backend

Jefferson deve collaborare strettamente con Jeorge per:
- sapere quali **endpoint** sono disponibili (URL, metodi HTTP);
- conoscere il **formato delle risposte** (JSON o HTML);
- gestire correttamente:
  - messaggi di errore;
  - codici di stato HTTP (200, 400, 422, ecc.).

Esempio di integrazione (scenario API JSON):
- `GET /polls` → usato per popolare la lista sondaggi;
- `GET /polls/{id}` → usato per mostrare dettagli e risultati;
- `POST /polls` → usato dalla pagina di creazione;
- `POST /polls/{id}/vote` → usato per inviare il voto;
- `POST/PATCH /polls/{id}/close` → potrebbe essere usato da un’interfaccia riservata al creatore (opzionale sul frontend se gestito altrove).

---

### 7. Task concreti per Jefferson

1. **Definire il layout generale**
   - Strutturare una pagina principale con intestazione, area contenuti e footer semplice.

2. **Implementare la lista sondaggi**
   - Creare una vista che chiama il backend (`GET /polls`) e mostra i dati in tabella o card.

3. **Implementare il dettaglio sondaggio**
   - Mostrare domanda, opzioni, risultati.
   - Implementare il form di voto e la gestione della risposta (successo/errore).
   - Integrare il polling per aggiornare i risultati.

4. **Implementare la creazione sondaggio**
   - Costruire il form con domanda e 3–5 opzioni.
   - Validare input lato client prima di inviare la richiesta.

5. **Migliorare la UX**
   - Aggiungere messaggi di stato, loader/spinner durante le richieste, gestione errori visibile.

---

### 8. Output atteso dal ruolo Frontend

A fine progetto, Jefferson dovrebbe consegnare:
- un’interfaccia completa che copre:
  - lista sondaggi,
  - creazione sondaggio,
  - voto,
  - visualizzazione risultati;
- una UI sufficientemente curata (layout, colori, leggibilità);
- un comportamento coerente con il backend:
  - uso corretto delle API;
  - gestione degli errori;
  - rispetto delle regole di business (nessun secondo voto, sondaggi chiusi non votabili).

