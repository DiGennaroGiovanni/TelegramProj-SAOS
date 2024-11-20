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

# 3. Configurazione ambiente - ngrok
Ngrok è uno strumento che consente di esporre server o applicazioni in esecuzione localmente su internet tramite un tunnel sicuro. È particolarmente utile durante lo sviluppo, per testare servizi come API, webhook o altre interfacce.
Caratteristiche principali:
- Fornisce un URL pubblico, accessibile da qualsiasi dispositivo connesso a internet, che inoltra le richieste al server locale.
- Supporta connessioni HTTPS, garantendo che i dati inviati attraverso il tunnel siano protetti durante il transito.
- Semplice da configurare e funziona senza modificare il firewall o la rete.

<h4>Istruzioni per installare e configurare ngrok:</h4>

1. Visitare il sito: https://ngrok.com/ e registrarsi.
 
Dopo la registrazione su ngrok, viene fornito un Authtoken (un codice alfanumerico unico). Questo token serve per autenticare l'utente e abilitare l'accesso alle funzionalità di ngrok, come l'utilizzo di URL persistenti o altre opzioni avanzate.

2. Scaricare l'eseguibile ngrok.exe ed aggiungerlo alle variabili d'ambiente in modo da poter eseguire i comandi successivi senza problemi:
  - Aggiungere il percorso di ngrok.exe manualmente alle variabili di ambiente Path,
  - in alternativa, aprire il prompt dei comandi windows e digitare
    `setx /M PATH "%PATH%;<PATH_TO_NGROK>"`  ad esempio :
    `setx /M PATH "%PATH%;C:\tools\ngrok"`

3. Dopo aver aggiunto ngrok alle variabili d'ambiente configurarlo con il proprio token ottenuto durante la fase di registrazione, eseguire quindi il seguente comando:
`ngrok config add-authtoken <YOUR_AUTHTOKEN>`

4. Per avviare il servizio di esposizione sulla porta 5000 eseguire sul prompt:
`ngrok http 5000`

5. Una volta avviato il servizio si otterrà una schermata tipo:
![ngrok](https://github.com/user-attachments/assets/be03b4b1-5577-4835-8ffb-8ecc7770f3cb)
Inserire quindi nel file <b>config.py</b> in <b>WEBHOOK_URL</b> il valore di <b>"Forwarding"</b> per ottenere una cosa come : `WEBHOOK_URL = 'https://51f7-87-10-197-93.ngrok-free.app/webhook'`

<b>ATTENZIONE:</b> Ogni volta che ngrok viene stoppato, bisogna rieseguire il comando al <b>punto 4</b> e modificare il file <b>config.py</b>

Ecco cosa fare con il codice e l'eseguibile .exe:
  `ngrok http 5000`
  Configurazione del webhook su Telegram con un esempio:
  - Monitoraggio delle richieste

# 4. Sicurezza di Telegram
- Uso di chat_id
  
# 5. Funzionalità sviluppate
5.1 Registrazione Utente
Descrizione di come l'utente si registra tramite Telegram.
5.2 Modifica Password
Flusso per aggiornare la password tramite la web app.
5.3 Eliminazione Profilo
Dettagli del processo per la rimozione dell'utente.
Dettagli Tecnici

# 6. Endpoint API
6.1 Elenco di tutti gli endpoint (es., /register, /update-password, /delete-profile) con spiegazioni dei parametri e delle risposte.
6.2 Struttura del webhook: Come il server Flask riceve richieste da Telegram.
6.3 Configurazione di Flask per gestire HTTPS con ngrok.

# 7. Testing
7.1 Come testare localmente utilizzando ngrok.
7.2 Verificare la connessione tra Telegram, il webhook e la web app.
