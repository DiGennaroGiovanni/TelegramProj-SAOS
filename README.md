# Registrazione e Gestione Profilo via Telegram 
Progetto per l'esame di Sicurezza delle Architetture Orientate ai Servizi.
Studente: Di Gennaro Giovanni

Il progetto verte alla realizzazione di un servizio di registrazione e gestione account (eliminazione e modifica password) basato su Telegram. La sicurezza di tale servizio è garantita sia dall'uso del protocollo HTTPS sul quale i dati viaggiano cifrati che sulla sicurezza offerta da Telegram.

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
Inserire nel file `config.py` in `WEBHOOK_URL` il valore di `"Forwarding"` per ottenere una cosa come : `WEBHOOK_URL = 'https://51f7-87-10-197-93.ngrok-free.app/webhook'`

6. Eseguire il comando `/setdomain` sulla chat di telegram con @BotFather inserendo il valore di Forwaring ottenuto (es. https://51f7-87-10-197-93.ngrok-free.app)
   
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

  
# 5. Funzionalità sviluppate
???


