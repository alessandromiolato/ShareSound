# ShareSound - Piattaforma per la condivisione di contenuti musicali


## DESCRIZIONE:
**ShareSound** è un sistema software progettato per gestire la collezione e la condivisione di materiale musicale di vario genere.  
Il sistema permette a differenti utenti autorizzati dall'amministratore di inserire, gestire e condividere informazioni e materiale digitale relativo ai brani musicali.

### Funzionalità  principali:
- Gestione completa di **brani musicali** con metadati (titolo, autore, esecutori, anno, genere, strumenti)
- Caricamento di **materiale digitale**: spartiti (PDF), file audio (MP3) e video (MP4)
- Integrazione con **YouTube** per caricamento e riproduzione di concerti
- Sistema di **commenti gerarchici** con risposte multi-livello
- Sistema di **note temporizzate** per brani e concerti
- **Motore di ricerca** per titolo, autore, esecutore e genere
- Gestione degli **utenti** con sistema di approvazione amministrativa


## ARCHITETTURA SOFTWARE:

### Pattern architetturali:
- **MVC (Model-View-Controller)**: Separazione tra logica di business, presentazione e controllo
- **DAO (Data Access Object)**: Astrazione dell'accesso ai dati
- **Singleton**: Per gestione centralizzata delle risorse (connessione database, contesto utente)

### Tecnologie utilizzate:
- **Linguaggio**: Java
- **UI Framework**: JavaFX
- **Database**: SQLite con connessione JDBC
- **Testing**: JUnit per unit e integration testing
- **Version Control**: Git/GitHub per sviluppo collaborativo


## STRUTTURA DEL DATABASE:

Il sistema utilizza un database relazionale SQLite con le seguenti entità principali:

- **Member**: Gestione utenti (username, email, password hash, ruolo, stato)
- **Track**: Informazioni sui brani musicali
- **Author**: Autori dei brani
- **Performer**: Esecutori dei brani
- **Genre**: Generi musicali predefiniti
- **Media**: File digitali associati ai brani (PDF, MP3, MP4)
- **Comment**: Sistema di commenti con struttura gerarchica (parent-child)
- **Note**: Note temporizzate per brani e concerti
- **Concert**: Concerti da YouTube con metadati
- **Concert_Data**: Informazioni dettagliate sui brani eseguiti nei concerti
- **Track_Performer**: Tabella di giunzione per relazione many-to-many tra brani ed esecutori


## FUNZIONALITÀ PER ATTORE:

### Utente:
- **Registrazione e autenticazione**: Richiesta di registrazione soggetta ad approvazione amministrativa
- **Gestione brani**: Inserimento di nuovi brani con tutti i metadati (titolo, autore, esecutori, anno, genere, strumenti, link YouTube, copertina)
- **Caricamento materiale**: Upload di documenti (PDF), file audio (MP3) e video (MP4)
- **Riproduzione/Download**: Visualizzazione e scaricamento del materiale digitale
- **Commenti**: Scrittura di commenti e risposte multi-livello; i commenti degli autori/esecutori sono evidenziati
- **Note temporizzate**: Aggiunta di annotazioni con timestamp per brani e concerti
- **Ricerca avanzata**: Ricerca filtrata per genere, titolo, autore ed esecutore
- **Gestione concerti**: Inserimento di concerti YouTube con metadati tecnici (brani eseguiti, strumenti, luogo, data)

### Amministratore:
- **Approvazione utenti**: Valutazione delle richieste di registrazione (accettazione/rifiuto)
- **Moderazione**: Ban di utenti problematici
- **Gestione contenuti**: Eliminazione di qualsiasi commento o contenuto inappropriato
- Tutte le funzionalità  dell'utente standard


## CARATTERISTICHE TECNICHE:

### Sistema di autenticazione:
- Password hashate utilizzando algoritmo sicuro
- Gestione degli stati utente: `PENDING` (in attesa di approvazione), `ACTIVE` (attivo), `BANNED` (bannato)
- Ruoli: `USER` e `ADMIN`

### Validazione input:
- Controlli rigorosi sui campi obbligatori durante l'inserimento dei brani
- Validazione dei link YouTube con pattern matching
- Controllo dei formati file (solo PDF per documenti, MP3/MP4 per multimedia)
- Validazione dell'anno di composizione con range realistico

### Sistema di commenti:
- Struttura gerarchica con supporto fino a 15 livelli di profondità
- Relazione parent-child con eliminazione a cascata
- Evidenziazione dei commenti di autori/esecutori del brano

### Ricerca intelligente:
- Algoritmo di ricerca per somiglianza
- Supporto per ricerche parziali e tolleranza agli errori
- Ricerca su multipli attributi simultaneamente


## TEST E QUALITÀ:

Il software è stato sottoposto a diversi livelli di testing:

### Unit Testing:
- **TrackDaoTest**: Test sui metodi di gestione brani (getter/setter, inserimento, ricerca)
- **MemberDaoTest**: Test sulla gestione utenti (creazione, ricerca, cambio stato)

### Integration Testing:
- **TrackDocumentIntegrationTest**: Test sull'integrazione tra brani e documenti associati
- **CommentSystemIntegrationTest**: Test sul sistema di commenti gerarchici fino a 15 livelli

### Test funzionali:
- Test su tutti i moduli principali (autenticazione, inserimento brani, gestione permessi, ricerca)
- Test con utenti generici per validazione UX e stabilità 

**Risultati**: Tutti i test hanno avuto esito positivo, confermando la robustezza e l'affidabilità del sistema.

## NOTE

**DATABASE:**  
Il file `appmusicale.db` non è incluso nel repository: viene creato automaticamente alla prima esecuzione.

**AMMINISTRATORE:**  
Non esistono credenziali di default. L'account admin si crea da terminale eseguendo la classe `com.appmusicale.AdminSignin` (username, email e password richiesti).  
- Utenti: devono registrarsi e attendere l'approvazione dell'amministratore

**YOUTUBE API:**  
Per poter utilizzare le funzionalità relative a Youtube, è necessario inserire la propria YOUTUBE_API_KEY.  
Da terminale: `export YOUTUBE_API_KEY=...`

**(Per documentazione completa consultare ["Relazione.pdf"](Relazione.pdf))**  

**REQUISITI:**  
- Java 21
- JavaFX 21 (dipendenza Maven)
- SQLite JDBC driver (dipendenza Maven)
- JUnit 5 (per esecuzione test: `mvn test`)
