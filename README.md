# Progetto Database: Piattaforma di Food Delivery "Cibora"

## Descrizione Generale del Progetto

Questo progetto riguarda la progettazione e la programmazione della base di dati per **Cibora**, un servizio di food delivery innovativo. Il sistema gestirà i dati relativi ai ristoranti affiliati, agli utenti, ai loro ordini e ai fattorini (riders) che effettuano le consegne.

## Entità Principali e Attributi

Di seguito sono descritte le entità principali del dominio applicativo e i loro attributi chiave, come specificato nei requisiti di progetto.

### 1. Utente

L'entità Utente rappresenta il cliente finale della piattaforma.

* **Dati Anagrafici**: Per la registrazione sono richiesti nome, email, password, numero di telefono e un indirizzo di recapito.
* **Borsellino Elettronico**: Ogni utente ha un borsellino con un saldo che viene aggiornato ad ogni ordinazione. L'utente può ricaricare il borsellino in qualsiasi momento.
* **Metodo di Pagamento**: L'utente deve associare un metodo di pagamento (es. carta di credito, PayPal) per poter ricaricare il borsellino.
* **Modalità Premium**: Gli utenti possono sottoscrivere un abbonamento "premium" che garantisce priorità sugli ordini.
* **Codici Sconto**: L'utente può accumulare codici sconto basati sul numero di ordini effettuati in passato.
* **Interazioni**: L'utente può visualizzare lo storico dei propri ordini, effettuare reclami inviando un messaggio al ristorante, e recensire ristoranti e rider.

### 2. Ristorante

Rappresenta l'esercizio commerciale che offre cibo sulla piattaforma.

* **Informazioni di Base**: Ogni ristorante è identificato da un nome, una descrizione, un indirizzo e un'immagine di profilo.
* **Costi**: Definisce un costo di spedizione per gli ordini.
* **Valutazione**: Possiede un numero di stellette che viene aggiornato ogni lunedì in base alla percentuale di recensioni positive della settimana precedente.
* **Categorie**: Appartiene a una o più categorie in base al tipo di cibo offerto (es. fast food, vegetariano).
* **Status: Top Partner**:
    * I ristoranti che offrono un servizio eccellente possono diventare "Top Partner".
    * I criteri sono: almeno 20 ordini corretti, valutazione >= 4.5, ordini annullati <= 1.5%, reclami <= 2.5%.
    * I Top Partner ricevono un badge speciale e maggiore visibilità.
    * Per i Top Partner viene memorizzata la data in cui hanno ottenuto tale status.

### 3. Piatto

Descrive le singole voci ordinabili dal menu di un ristorante.

* **Attributi**: Ogni piatto ha un titolo, un'immagine, una lista di ingredienti, una lista di allergeni e un prezzo.
* **Promozioni**: Può avere uno sconto applicato al prezzo di listino.
* **Liste**: Ogni piatto può appartenere a una o più liste speciali (es. "i più venduti", "promozioni").

### 4. Rider (Fattorino)

È l'incaricato della consegna degli ordini.

* **Identificazione**: Ogni rider è identificato da un codice univoco.
* **Stato**: Il suo stato può essere "occupato", "disponibile" o "fuori servizio".
* **Posizione**: La sua posizione viene aggiornata in tempo reale tramite GPS.
* **Mezzo di Trasporto**: I riders sono classificati in base al mezzo utilizzato: bicicletta normale, bicicletta elettrica, o monopattino.
* **Autonomia Monopattino**: Chi usa il monopattino deve specificare i km di autonomia della batteria.
* **Performance**: Per ogni rider si tiene traccia del numero totale di consegne effettuate.

### 5. Ordine

Rappresenta la transazione tra un utente e un ristorante.

* **Contenuto**: È composto da una lista di piatti selezionati da un utente.
* **Stato**: Un ordine può essere annullato dal cliente o dal ristoratore finché non viene affidato a un rider.
* **Tracciamento**: Vengono memorizzati i seguenti timestamp:
    * Momento in cui l'ordine viene affidato a un rider.
    * Ora di effettiva consegna al cliente (per gli ordini completati).
* **Interazioni Post-Ordine**: Dopo la consegna, l'utente può recensire ristorante e rider, e lasciare una mancia a quest'ultimo.

## Funzionalità Specifiche e Processi

### Assegnazione del Rider
Il sistema di assegnazione ordini segue regole precise:
1.  Viene individuato il rider libero la cui somma delle distanze `(rider -> ristorante) + (ristorante -> utente)` è minima.
2.  Per tragitti totali `(rider -> ristorante -> cliente)` superiori a 10 km, vengono considerati solo i rider con bici elettrica.

### Interazioni e Comunicazione
* **Chat**: L'utente ha la possibilità di chattare con il ristorante e con il rider in caso di problemi con l'ordine.
* **Reclami**: Gli utenti possono inviare un messaggio al ristorante per segnalare un problema tramite la sezione degli ordini passati.
* **Recensioni**: Dopo la consegna, l'utente può lasciare una valutazione da 1 a 5 stelle e un commento testuale (facoltativo) sia per il ristorante che per il rider.

### Classifiche Mensili
Una volta al mese, il sistema aggiorna le seguenti classifiche:
* Riders più veloci nel consegnare gli ordini.
* Cibi più popolari.
* Ristoranti con più recensioni positive.
* Clienti che hanno speso di più.
