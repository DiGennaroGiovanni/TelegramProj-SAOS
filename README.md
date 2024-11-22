# Registrazione e Gestione Profilo via Telegram 
Progetto per l'esame di Sicurezza delle Architetture Orientate ai Servizi.

Studente: Di Gennaro Giovanni

Il progetto verte alla realizzazione di un servizio di registrazione e gestione account basato su Telegram. La sicurezza di tale servizio è garantita sia dall'uso del protocollo HTTPS sul quale i dati viaggiano cifrati che sulla sicurezza offerta da Telegram.

# Principali tecnologie utilizzate:
- Firestore Database
- PyCharm / Python
- HTML / CSS / /JavaScript
- Telegram
- Flask
- Ngrok
- WebHook

# 1. Architettura del servizio
    INSERIRE IMMAGINE
    
# 2. Creazione chatBot telegram
I chatbot sono piccole applicazioni che vengono eseguite interamente all'interno dell'app Telegram. Gli utenti interagiscono con i bot attraverso interfacce flessibili che possono supportare qualsiasi tipo di compito o servizio.

Alcune funzionalità dei bot:
- Sostituire interi siti web
- Gestire la propria attività
- Ricevere pagamenti
- Creare strumenti personalizzati
- Integrazione con servizi e dispositivi

<h3>ISTRUZIONI PER CREARE E CONFIGURARE IL PROPRIO CHATBOT:</h3>

1. Visitare il bot ufficiale di Telegram `@BotFather`
2. Avviare il Bot e quindi inviare il comando `/newbot`. Seguire le istruzioni per la creazione del bot.

Al termine della creazione, verrà fornito un <b>TOKEN</b> univoco, come ad esempio `123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11`. Sarà molto importante tenere questo TOKEN al sicuro e non condividerlo in quanto permette di gestire il Bot mediante le richieste API che potranno essere eseguite con `https://api.telegram.org/bot<token>/METHOD_NAME` o mediante comandi in chat (sempre usando il @BotFather)

3. Aggiungere il TOKEN fornito nel file `config.py` nella voce `TELEGRAM_TOKEN`
4. Nel file `templates/index1.html` modificare la riga `data-telegram-login="NOME_BOT" ` inserendo il nome del proprio Bot.

<h3>CONFIGURAZIONE COMANDO UTILE AL PROGETTO:</h3>
Il servizio realizzato in questo progetto utilizza il chatBot per modificare alcune informazioni dell'utente. A tal proposito è necessario creare un comando personalizzato.

1. Inviare il comando `/setcommands` all'interno della chat con @BotFather
2. Creare esattamente il comando `indirizzofatturazione - Inserisci o modifica il tuo indirizzo di fatturazione`

# 3. Configurazione ambiente - ngrok
Ngrok è uno strumento che consente di esporre server o applicazioni in esecuzione localmente su internet tramite un tunnel sicuro. 
È particolarmente utile durante lo sviluppo, per testare servizi come API, webhook o altre interfacce.
Caratteristiche principali:
- Fornisce un URL pubblico, accessibile da qualsiasi dispositivo connesso a internet, che inoltra le richieste al server locale.
- Supporta connessioni HTTPS, garantendo che i dati inviati attraverso il tunnel siano protetti durante il transito.
- Semplice da configurare e funziona senza modificare il firewall o la rete.

<h3>ISTRUZIONI PER INSTALLARE E CONFIGURARE NGROK:</h3>

1. Visitare il sito: https://ngrok.com/ e registrarsi.
 
Dopo la registrazione su ngrok, viene fornito un Authtoken (un codice alfanumerico unico). Questo token serve per autenticare l'utente e abilitare l'accesso alle funzionalità di ngrok, come l'utilizzo di URL persistenti o altre opzioni avanzate.

2. Scaricare l'eseguibile ngrok.exe ed aggiungerlo alle variabili d'ambiente in modo da poter eseguire i comandi successivi senza problemi:
  - Aggiungere il percorso di ngrok.exe manualmente alle variabili di ambiente Path,
  - in alternativa, aprire il prompt dei comandi windows e digitare
    `setx /M PATH "%PATH%;<PATH_TO_NGROK>"`  ad esempio :
    `setx /M PATH "%PATH%;C:\tools\ngrok"`

3. Dopo aver aggiunto ngrok alle variabili d'ambiente, configurarlo con il token ottenuto durante la fase di registrazione, eseguire quindi il seguente comando:
`ngrok config add-authtoken <YOUR_AUTHTOKEN>`

4. Per avviare il servizio di esposizione sulla porta 5000 eseguire sul prompt:
`ngrok http 5000`

5. Una volta avviato il servizio si otterrà una schermata tipo:
![ngrok](https://github.com/user-attachments/assets/be03b4b1-5577-4835-8ffb-8ecc7770f3cb)
Inserire nel file `config.py` in `WEBHOOK_URL` il valore di `"Forwarding"` per ottenere una cosa come :
`WEBHOOK_URL = 'https://51f7-87-10-197-93.ngrok-free.app/webhook'`

7. Eseguire il comando `/setdomain` sulla chat di telegram con @BotFather inserendo il valore di Forwaring ottenuto (es. https://51f7-87-10-197-93.ngrok-free.app)
   
<b>ATTENZIONE:</b> Ogni volta che ngrok viene stoppato, bisogna rieseguire il comando dell <b>punto 4</b> e modificare sia il file <b>config.py</b> che il dominio del bot al <b>punto 6</b>

# 4. Webhook

Un webhook è un metodo per ricevere notifiche o aggiornamenti in tempo reale da un servizio. Funziona inviando automaticamente una richiesta HTTP (solitamente POST) a un endpoint specifico quando si verifica un determinato evento.

<b>Come funziona un webhook:</b>
- Registrazione dell'endpoint: Il server del servizio (in questo caso, Telegram) deve sapere dove inviare le notifiche. Per farlo, viene eseguita la funzione `def async set_bot_webhook()` nel `main.py`
- Notifica dell'evento: Quando si verifica un evento (ad esempio, un utente invia un messaggio al bot), Telegram invia una richiesta POST all'URL registrato. Questa richiesta contiene i dettagli dell'evento in formato JSON.
- Elaborazione della richiesta: Il server riceve la richiesta e la gestisce in base alla logica implementata come ad esempio inviando una risposta tramite l'API di Telegram.
- Come interagisce un webhook con Telegram:
Telegram utilizza il webhook per inviare gli aggiornamenti relativi al bot, come messaggi, callback delle tastiere inline, comandi e altro.

Una volta impostato un webhook con l'API di Telegram, non è più necessario eseguire il polling (ovvero interrogare periodicamente il server di Telegram per nuovi messaggi). Questo rende il sistema più efficiente e reattivo.

Per monitorare le richieste inviate al server è possibile accedere al link : `http://localhost:4040/inspect/http`

# 5. Configurazione Firebase - Firestore Database

Firestore è un database NoSQL orientato ai documenti. A differenza di un database SQL, non ci sono tabelle o righe. I dati vengono archiviati in documenti, che sono organizzati in raccolte.
Ogni documento contiene un insieme di coppie chiave-valore. Firestore è ottimizzato per l'archiviazione di grandi raccolte di piccoli documenti.
Tutti i documenti devono essere archiviati in raccolte.
Le raccolte e i documenti vengono creati implicitamente in Firestore, è sufficiente assegnare i dati a un documento all'interno di una raccolta. Se la raccolta o il documento non esistono, Firestore li crea.

<b>DOCUMENTI</b>

In Firestore, l'unità di archiviazione è il documento. Un documento è un un record leggero contenente campi mappati a valori. Ogni documento è identificati da un nome.
Un documento che rappresenta l'utente alovelace potrebbe avere il seguente aspetto:

![image](https://github.com/user-attachments/assets/4f30ac6b-b528-4856-adbe-8856f320b11c)

<b>RACCOLTE</b>

I documenti si trovano nelle raccolte, che sono semplicemente contenitori per i documenti. Per Ad esempio, potresti avere una raccolta users che contiene i vari utenti, rappresentati da un documento:

![image](https://github.com/user-attachments/assets/743322cc-a3ee-409a-94f7-de7f0a3d12d0)

<h3>CONFIGURAZIONE DATABASE:</h3>

1. Dopo aver fatto l'accesso tramite Google al sito `https://console.firebase.google.com/u/0/`, creare un nuovo progetto seguendo le istruzioni.
2. 
   <b>ATTENZIONE:</b> Ai fini del progetto, si raccomanda di non impostare <b>nessuna regola</b> per evitare errori di permessi durante la scrittura sul database
3. Accedere al progetto appena creato e creare una nuova raccolta `'users'`

Per salvare le credenziali di accesso da inserire all'interno del file `config.py` seguire i seguenti passaggi: 

1. Accedere alla sezione `Impostazioni progetto`:
   
![ImpostazioniProgetto](https://github.com/user-attachments/assets/40362d5c-37f3-4ed5-9048-b24c3503ea9e)

2. Andare nella sezione `Account di servizio` e quindi `SDK Firebase Admin`
3. Selezionare `Python` come `Snippet di configurazione SDK Admin`
4. Cliccare su `Genera nuova chiave privata`

Verrà scaricato un file .json contenete le informazioni necessarie al collegamento del database. 

5. Inserire il file appena scaricato all'interno della cartella del progetto 
6. Modificare il valore `FIREBASE_CREDENTIALS` all'interno del file `config.py` inserendo il nome completo del file scaricato precedentemente

# 6. Flask

Flask è un framework web leggero, più precisamente un microframework, open-source per il linguaggio di programmazione Python. È progettato per facilitare la creazione di applicazioni web, offrendo gli strumenti di base necessari per gestire richieste HTTP, routing, gestione delle sessioni e rendering di template.

Flask è particolarmente utile per:

- <b>Sviluppare API web:</b> grazie alla sua semplicità e flessibilità è molto usato per costruire API RESTful.
- <b>Creare siti web dinamici:</b> Consente di generare pagine web dinamiche, interagendo con il database, gestendo la logica di business e rendendo il contenuto personalizzato per gli utenti.

Vantaggi nell'utilizzo di Flask

- <b>Flessibilità:</b> Essendo un microframework, Flask non impone una struttura rigida e consente agli sviluppatori di scegliere le librerie o gli strumenti che meglio si adattano al progetto. È possibile aggiungere facilmente estensioni per funzionalità come la gestione delle sessioni, la convalida dei moduli e l'autenticazione.

- <b>Scalabilità:</b> Sebbene sia un framework leggero, Flask è abbastanza scalabile per supportare applicazioni più complesse. Può essere utilizzato per progetti di piccole dimensioni così come per applicazioni più grandi, con una gestione del codice che resta relativamente semplice.

# 6. Funzionalità sviluppate e controlli di sicurezza

Per testare le funzionalità dell'applicazione, bisogna visitare il link ottenuto nella <b>SEZIONE 3, PUNTO 5 (WEBHOOK_URL)</b>

<h3>REGISTRAZIONE e LOGIN</h3>

Queste due funzionalità sono implementate attraverso il `Telegram Login Widget`. Quando si utilizza il login Telegram per la prima volta, il widget chiede il numero di telefono all'utente e invia un messaggio di conferma via Telegram per autorizzare il browser.
Una volta fatto questo, viene visualizzato un meccanismo di <b>two-click login</b> sul sito.

Quando si esegue l'accesso, verrà inviato il nome di Telegram, nome utente e la foto profilo al sito web. Il numero di telefono rimane invece nascosto. Il sito web può anche richiedere l'autorizzazione per inviare messaggi dal loro bot.

Dopo ogni login, Telegram invierà un messaggio di riepilogo delle autorizzazioni concesse e dei dati che il sito web possiede. È possibile revocare l'autorizzazione toccando il pulsante appropriato sotto la sintesi di accesso.

![Richiesta1_telegram](https://github.com/user-attachments/assets/eaffb063-cd2e-4117-9d25-c9b72a678144)

![Richiesta2_telegram](https://github.com/user-attachments/assets/2df6cb88-b883-4298-9ba9-30583718ade2)

![Richiesta3_telegram](https://github.com/user-attachments/assets/04bbe999-4b56-4da3-b65c-5c931e2c3a45)

Dopo aver concesso le autorizzazioni, il widget restituisce i dati in due modi.
Ai fini di questo progetto i dati vengono recuperati attraverso un JSON contenente i campi <b>id, first_name, last_name, username, photo_url, auth_date e hash</b>.

Visitando `http://localhost:4040/inspect/http` si potrà vedere la richiesta `POST /auth` (/auth è l'endopoint del webHook) inviata al server come la seguente, con lo stato `200 OK`:

![json_login](https://github.com/user-attachments/assets/3ae260a8-35ba-4698-a54b-19189f5665b0)

<b>ASPETTI DI SICUREZZA: Uso di OAuth da parte di Telegram</b>

OAuth (Open Authorization) è uno standard aperto per l'autorizzazione sicura che consente a un'applicazione di accedere a risorse protette su un altro servizio senza dover condividere le credenziali (come nome utente e password). In altre parole, OAuth permette a un'app di agire per conto dell'utente in modo sicuro e controllato.

OAuth si basa su un modello che coinvolge tre parti principali:

- <b>Utente:</b> La persona che concede l'autorizzazione per accedere a una risorsa (in questo caso l'utente che visita la webApp)
- <b>Applicazione client:</b> Il servizio o l'app che richiede l'accesso alle risorse dell'utente (in questo caso la webApp).
- <b>Server di risorse:</b> Il servizio che ospita le risorse protette e verifica se il client ha l'autorizzazione per accedervi (in questo caso Telegram).

<b>ASPETTI DI SICUREZZA: Verifica della firma HMAC</b>

Telegram fornisce una hash che è una firma crittografica generata utilizzando una chiave segreta basata sul token del bot. Il server ricostruisce una stringa di controllo (data_check_string) contenente i dati ricevuti, ordinati in ordine alfabetico (come specificato nella documentazione ufficiale di Telegram). Utilizza l'algoritmo HMAC-SHA256 e il token del bot per calcolare la propria firma (hmac_signature).
Se la firma calcolata corrisponde a quella fornita (hash_received), i dati sono considerati autentici e vengono salvati sul database (se l'utente non esiste) e passati nella sessione di Flask.

<b>Vantaggi di questo approccio</b>
- <b>Sicurezza:</b> L'uso della firma HMAC garantisce che i dati non vengano manomessi.
- <b>Semplicità:</b> Gli utenti possono autenticarsi con un clic, senza necessità di password o registrazione manuale.
- <b>Integrazione con database:</b> I dati degli utenti vengono salvati in Firestore, facilitando la gestione di utenti registrati.

<h3>MODIFICA UTENTE</h3>
 
Dopo aver effettuato il login, i dati dell'utente vengono salvati nella sessione di Flask e resi disponibili nella pagina dashboard tramite la variabile user_data.

Il frontend utilizza JavaScript per popolare la pagina HTML con i dati utente (nome, username Telegram, chat ID, indirizzo di fatturazione, ecc.).

Qui è stata implementata una funzionalità esemplificativa di modifica dei dati dell'utente mediante il pulsante `Modifica Profilo`.

Quando l'utente clicca su tale profilo, viene inviata una richiesta `POST /modify_account` (/modify_account è la rotta) con lo stato `200 OK` in caso di successo.

La rotta `/webhook` nel backend Flask gestisce le richieste inviate al bot Telegram, quindi controlla il tipo di payload ricevuto:

Se il payload contiene i campi username, action, e chat_id, questi vengono processati: l'azione <b>"modify"</b> viene rilevata.
Il bot verifica se sono configurati comandi disponibili per l’utente (Comandi impostati nella <b>SEZIONE 2 in "CONFIGURAZIONE COMANDO UTILE AL PROGETTO", punto 2</b>).

L'utente riceve quindi un messaggio dal Bot di Telegram (configurato nella SEZIONE 2):

![bot_commands](https://github.com/user-attachments/assets/f6298973-dd07-4fe5-95fc-753d536e71cc)

Per questo progetto è possibile eseguire solo il comando `/indirizzofatturazione`, inviando tale comando al Bot, verrà avviato il processo di modifica come segue:

![bot_commands1](https://github.com/user-attachments/assets/b8a18319-4cad-44a9-a807-be4ec2a38830)

Lo scambio dei messaggi e l'acquisizione del nuovo indirizzo avviene mediante il controllo dei `callback` e dei `testi` presenti nelle richieste inviate ed intercettate dal webhook:

Parte del <b>Callback</b> relativo alla scelta dell'utente:

![callback_bot](https://github.com/user-attachments/assets/49f3427d-e681-47e7-9db1-622db4967b71)

<b>POST contenente il messaggio dell'utente</b>

![text_bot](https://github.com/user-attachments/assets/d895d533-7fc7-4b4f-b00f-51b180cbcc4e)

Se tutto il processo va a buon fine, il nuovo indirizzo viene salvato sul database ed è possibile visualizzarlo nella dashboard aggiornando la pagina. 

<h3>ASPETTI DI SICUREZZA: Impossibilità di accedere alla dashboard senza login</h3>

Per evitare che un utente riesca ad accedere all'endpoint della dashboard senza aver effettivamente eseguito la registrazione/login, vengono controllati i dati di sessione lato server. Nel caso in cui questi siano vuoti ed un utente visita direttamente `WEBHOOK_URL/dashboard`, verrà reindirizzato in `WEBHOOK_URL/`


<h3>LOGOUT</h3>

La funzione di logout segue un flusso semplice e lineare che si articola in due parti principali: la parte frontend (JavaScript) e la parte backend (Python).
Quando un utente clicca su `Logout`, viene invocato l'endpoint `/logout` con il metodo `POST`, anche in questo caso, si otterrà un valore di stato `200 OK`.
Specifica `credentials: 'same-origin'` per assicurarsi che i cookie di sessione vengano inviati con la richiesta, garantendo che l'utente autenticato possa essere identificato dal server.
Vengono rimossi quindi tutti i dati contenuti in `user_data` dalla sessione.
