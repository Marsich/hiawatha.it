# HIAWATHA

#### NOME

hiawatha - webserver avanzato e sicuro

- - - -

#### SINOSSI

hiawatha [opzioni]

```
Opzioni:
          -c <percorso>: percorso dove sono posizionati i files di configurazione.
          -d: non eseguire in background.
          -k: verifica la configurazione ed esci.
          -h: visualizza l'help ed esci.
          -v: visualizza la versione e le informazioni di compilazione ed esci.
```

- - - - -

#### DESCRIZIONE

Hiawatha è un webserver per Unix orientato alla sicurezza. E' stato scritto con in mente gli
obbiettivi di 'essere sicuro' e 'facile da usare'.
Hiawatha ha tantissime caratteristiche che nessun altro webserver ha.
Sebbene la maggior parte siano iniziate come un esperimento, molre di loro hanno dimostrato di essere piuttosto efficaci.

Hiawatha è stato testato e funziona perfettamente su sistemi Linux, BSD, MacOS X e Cygwin.

- - - -

#### FILES DI CONFIGURAZIONE

Hiawatha dispone dei seguenti files di configurazione:

`cgi-wrapper.conf`<br/>
Cfr. cgi-wrapper per ulteriori informazioni.

`hiawatha.conf`<br/>
Cfr. le sezioni SERVER CONFIGURATION, BINDING CONFIGURATION, VIRTUAL HOST CONFIGURATION, DIRECTORY CONFIGURATION, FASTCGI CONFIGURATION, URL TOOLKIT e XSLT per ulteriori informazioni.

`mimetype.conf`<br/>
Cfr. il capitolo MIMETYPES per ulteriori informazioni.

`.hiawatha`<br />
Cfr. il capitolo USER SETTINGS PER DIRECTORY per ulteriori informazioni.

- - - -

#### SEZIONI


Le cofigurazioni relative a binding, directory, FastCGI, virtual host e URL toolkit devono essere posizionati dentro a delle sezioni. Una sezione è definita come segue:

```
Section {
       ...

}
```

dove la parola "Section" deve essere sostituita con "Binding", "Directory", "FastCGIserver", "VirtualHost" oppure "UrlToolkit".

- - - -

#### CONFIGURAZIONE DEL SERVER

Il file di configurazione globale del server Hiawatha.

**set variabile = valore**<br/>
Con l'istruzione 'set', possiamo dichiarare una variabile. Assicuriamoci che il nome della variabile non vada
in conflitto con qualsiasi altra opzione di configurazione.
Le variabili sono case-sensitive e non possono essere re-dichiarate.<br/>
Esempio: set local_net = 192.168.1.0/24<br/>
AccessList = allow local_net, deny 0.0.0.0/0<br/>
(cfr AccessList per ulteriori informazioni su questa opzione)

- - - -

**AnonymizeIP = yes | no**<br/>
Anonymize gli indirizzi IP prima di scriverli nei log degli accessi e degli errori o prima di inviarli allo Hiawatha Monitor.<br/>
Default = no<br/>
Esempio: AnonymizeIP = yes

- - - -

**BanlistMask = (allow | deny) &lt;ip-address&gt;[/netmask][, (allow | deny) &lt;ip-address&gt;[/netmask], ...]**<br/>
Impedisce che degli indirizzi IP siano bannati in caso di comportamento scorretto.<br/>
Di default, tutti gli IP possono essere bannati. Gli IP che sono 'denied' dalla banlist non saranno bannati.<br/>
Esempio: BanlistMask = allow 192.168.1.2, deny 192.168.0.0/16

- - - -

**BanOnDeniedBody = &lt;ban-time&gt;**<br/>
Numero di secondi di blocco ('ban') di un IP in caso 'request body' vietato. Vedi anche [DenyBody](#DenyBody).<br />
Default = 0<br />
Esempio: BanOnDeniedBody = 120

- - - -

**BanOnFlooding = &lt;number&gt;/&lt;time&gt;:&lt;ban-time&gt;**<br />
Quando un client invia più di &lt;number&gt; richieste in &lt;time&gt; secondi, l'IP sarà bannato per &lt;ban-time&gt; secondi.<br />
Default = -/-:0<br />
Esempio: BanOnFlooding = 10/1:15

- - - -

**BanOnGarbage = &lt;ban-time&gt;**<br />
Durata in secondi del blocco ('ban') di un IP in caso di una richiesta HTTP malformata (400 Bad Request).<br />
I browsers normalmente non inviano mai una richiesta HTTP malformata.<br />
Quindi in caso di codice errore 400, probabilmente qualcuno sta tentando di fare qualcosa di non molto simpatico.<br />
Default = 0<br />
Esempio: BanOnGarbage = 60

- - - -

**BanOnInvalidURL = &lt;ban-time&gt;**<br/>
Numero di secondi di blocco di un IP in caso di un URL non valido.<br />
Default = 0<br />
Esempio: BanOnInvalidURL = 60

- - - -

**BanOnMaxPerIP = &lt;ban-time&gt;**<br />
Definisce per quanti secondi un client dovrà essere bloccato quando dovesse superare il numero massimo di connessioni simultanee.<br />
Vedi anche [ConnectionsPerIP](#ConnectionsPerIP).<br />
Default = 2<br />
Esempio: BanOnMaxPerIP = 5

- - - -

**BanOnMaxReqSize = &lt;ban-time&gt;**<br />
Definisce per quanti secondi un IP dovrà essere bloccato nel caso di una richiesta HTTP troppo grande (413 Request Entity Too Large).<br />
Vedi anche MaxRequestSize.<br />
Default = 0<br />
Esempio: BanOnMaxReqSize = 10

- - - -

**BanOnSQLi = &lt;ban-time&gt;**<a id="BanOnSQLi"></a><br />
Definisce per quanti secondi un IP dovrà essere bloccato nel caso venga rilevato un tentativo di SQL-injection.<br />
Vedi anche [PreventSQLi](#PreventSQLi).<br />
Default = 0<br />
Esempio: BanOnSQLi = 60

- - - -

**BanOnTimeout = &lt;ban-time&gt;**<br />
Definisce per quanti secondi un IP dovrà essere bloccato nel caso di un timeout prima che la prima richiesta sia stata inviata.<br />
Vedi anche TimeForRequest.<br />
Default = 0<br />
Esempio: BanOnTimeout = 30

- - - -

**BanOnWrongPassword = &lt;number&gt;:&lt;ban-time&gt;**<br />
Definisce per quanti secondi un IP dovrà essere bloccato nel caso di un numero &lt;number&gt; di password errate durante un'autenticazione HTTP.<br/>
Default = -:<br />
Esempio: BanOnWrongPassword = 3:120

- - - -

**CacheRProxyExtensions = &lt;estensione&gt;[, &lt;estensione&gt;, ...]**<br />
Abilita la cache interna per le richieste reverse proxy per queste estensioni.<br />
Esempio: CacheRProxyExtensions = css, gif, html, jpg, js, png, txt<br />
(E' necessario che Hiawatha NON sia stato compilato con `-DENABLE_CACHE=off` oppure con  `-DENABLE_RPROXY=off`)

- - - -

**CacheSize = &lt;dimensione in megabytes&gt;**<br />
Dimensione del file di cache interno di Hiawatha. La dimensione massima è 100 (megabytes).<br />
Default = 10<br />
Esempio: CacheSize = 25<br />
(E' necessario che Hiawatha NON sia stato compilato con `-DENABLE_CACHE=off`)

- - - -

**CacheMaxFilesize = &lt;dimensione in kilobytes&gt;**<br/>
Dimensione massima di un file in cui Hiawatha memorizzerà la propria cache interna.<br/>
Default = 256<br/>
Esempio: CacheMaxFilesize = 128<br/>
(E' necessario che Hiawatha NON sia stato compilato con `-DENABLE_CACHE=off`)

- - - -

**CGIextension = &lt;estensione&gt;[, &lt;estensione&gt;, ...]**<br/>
Estensioni di default di un programma CGI.<br/>
Esempio: CGIextension = cgi

- - - -

**CGIhandler = &lt;CGI handler&gt;:&lt;extension&gt;[, &lt;extension&gt;, ...]**<br />
Specifica il gestore ('handler') per una certa estensione CGI. Un gestore è un eseguibile che 'eseguirà' lo script CGI.<br />
Esempio: CGIhandler = /usr/bin/php4-cgi:php,php4

- - - -

**CGIwrapper = &lt;CGI wrapper&gt;**<br />
Specifica il wrapper per processi CGI. Un CGI wrapper sicuro è incluso nel pacchetto Hiawatha<br />
(cfr. cgi-wrapper per ulteriori informazioni).<br />
Default = /usr/sbin/cgi-wrapper<br />
Esempio: CGIwrapper = /bin/cgi-wrapper

- - - -

**ConnectionsPerIP = &lt;numero&gt;**<a id="ConnectionsPerIP"></a><br />
Numero massimo di connessioni simultanee per IP.<br />
Default = 10<br />
Esempio: ConnectionsPerIP = 5

- - - -

**ConnectionsTotal = &lt;numero&gt;**<br />
Numero massimo di connessioni simultanee.<br />
Default = 100<br />
Esempio: ConnectionsTotal = 250

- - - -

**DHsize = 1024|2048|4096**<br />
Configura la dimensione della chiave Diffie-Hellman.<br />
Default = 2048<br />
Esempio: DHsize = 4096

- - - -

**ExploitLogfile = &lt;filename con percorso completo&gt;**<br />
Logfile per tutti i tentativi di exploit: CSRF, denied bodies, SQL injection and XSS<br />
Default = /var/log/hiawatha/exploit.log<br />
Esempio: ExploitLogfile = /var/log/exploit_attempts.log

- - - -

**GarbageLogfile = &lt;filename con percorso completo&gt;**<br />
File di log da usare per tutte le richieste HTTP malformate.<br />
Esempio: GarbageLogfile = /var/log/hiawatha/garbage.log

- - - -

**HideProxy = &lt;indirizzo-ip&gt;[/netmask][, &lt;indirizzo-ip&gt;[/netmask], ...]**<br />
Una richiesta inviata dall'IP fornito sarà ricercata in una intestazione X-Forwarded-For.<br />
Quando trovata, l'ultimo indirizzo IP di quel campo sarà utilizzato come indirizzo IP del client.<br />
Assicuriamoci di utilizzare solo 'reverse proxies' affidabili in questa lista di IP.<br />
Questa opzione non influenza il settaggio dell'opzione [ConnectionsPerIP](#ConnectionsPerIP).<br />
Esempio: HideProxy = 192.168.10.20

- - - -

**Include &lt;filename&gt;|&lt;cartella&gt;**<br />
Include un altro file di configurazione o i files di configurazione in una cartella.<br />
Esempo: Include /etc/hiawatha/hosts.conf

- - - -

**KickOnBan = yes|no**<br />
In caso di blocco ('ban'), chiude tutte le ulteriori connessioni che arrivano dallo stesso IP.<br />
Default = no<br />
Esempio: KickOnBan = yes

- - - -

**KillTimedoutCGI = yes|no**<br />
Se un processo CGI va in timeout (vedi TimeForCGI per ulteriori informazioni),
Hiawatha invierà un segnale TERM al processo CGI, aspetterà 1 secondo e poi invierà un segnale KILL al processo CGI.<br />
Questa opzione non ha effetto su job FastCGI.<br />
Default = yes<br />
Esempio: KillTimedoutCGI = no

- - - -

**LogfileMask = (allow|deny) &lt;indirizzo-ip&gt;[/netmask][, (allow|deny) &lt;indirizzo-ip&gt;[/netmask], ...]**<br />
Definisce un elenco di IP le cui richieste HTTP saranno registrate nel log.<br />
Se un IP non si combina ('match') con gli IP elencati allora la richiesta sarà registrata nel log.<br />
Esempio: LogfileMask = deny 10.0.0.0/24

- - - -

**LogFormat = hiawatha|common|extended**<br />
Definisce il formato di un file di log:

hiawatha = formato di default usato da Hiawatha;<br />
common = Common Log Format;<br />
extended = Extended Common Log Format.

Default = hiawatha<br />
Esempio: LogFormat = extended

- - - -

**MaxServerLoad = &lt;valore&gt;**<br />
Quando il server è sottoposto ad un carico maggiore di &lt;valore&gt;,
Hiawatha lascerà cadere le connessioni in ingresso. Questa opzione è disponibile solo con Linux.<br />
Esempio: MaxServerLoad = 0.7

- - - -

**MaxUrlLength = &lt;valore&gt;**<br />
La lunghezza massima di una URL accettata dal webserver come valida.
In caso contrario (non accettata) Hiawatha ritornerà un codice di errore 414.
Questa verifica è disabilitata se &lt;valore&gt; è definito come 'none'.<br />
Default = 1000<br />
Esempio: MaxUrlLength = 500

- - - -

**MimetypeConfig = &lt;file_di_configurazione&gt;**<br />
Percorso del file di configurazione del mimetype.
Se il percorso è omesso, verrà usata la cartella del file di configurazione di Hiawatha.<br />
Default = mimetype.conf<br />
Esempio: MimetypeConfig = /etc/mime.types

- - - -

**MinSSLversion = SSL3.0|TLS1.0|TLS1.1|TLS1.2**<br />
Specifica la versione minima di SSL per le connessioni HTTPS.
Al di sotto di questa versione Hiawatha rifiuterà la connessione.<br />
Default = TLS1.0<br />
Esempio: MinSSLversion = TLS1.1<br />
(è necessario che Hiawatha sia compilato con `-DENABLE_SSL=on`)

- - - -

**MonitorServer = &lt;indirizzo-ip&gt;**<br />
Specifica l'indirizzo IP del server monitor. Questo abilita la registrazione di informazioni statistiche.
Usa un header CGI X-Hiawatha-Monitor per registrare un evento.<br />
Esempio: MonitorServer = 192.168.1.2

- - - -

**PIDfile = &lt;filename&gt;**<br />
Il nome del file in cui Hiawatha scriverà il proprio process-ID (PID).
Non lo dovremmo cambiare a meno di sapere esattamente quello che stiamo facendo
(il CGI-wrapper e il pannello delle preferenze di MacOS X si aspettano di trovare il PID file nella locazione di default).<br />
Default = /var/run/hiawatha.pid<br />
Esempio: PIDfile = /data/hiawatha.pid

- - - -

**Platform = cygwin | windows** <br />
Se settato a 'windows', Hiawatha convertirà lo Unix-style del percorso ai programmi CGI in un percorso Windows-style.<br />
Default = windows<br />
Esempio: Platform = cygwin<br />
(Questa opzione e disponibile solo nella versione Windows (Cygwin) di Hiawatha).

- - - -

**RebanDuringBan = yes|no**<br />
Resetta (azzera) il ban timer quando un client tenta di riconnetersi durante un ban.<br />
Default = no<br />
Esempio: RebanDuringBan = yes

- - - -

**ReconnectDelay = &lt;time&gt;**<br />
Definisce per quanti secondi Hiawatha terrà in memoria l'indirizzo IP della connessione supponendo di conseguenza che il client sia ancora connesso.<br />
In combinazione con [ConnectionsPerIP](#ConnectionsPerIP), questo settaggio può essere usato per prevenire il flood.<br />
Nota che sarà usato il ban timer del BanOnMaxPerIP, non quello del BanOnFlooding.<br />
Questa opzione potrebbe generare un po' di carico extra al server.<br />
Default = 0<br />
Esempio: ReconnectDelay = 3

- - - -

**RequestLimitMask = (allow|deny) &lt;indirizzo-ip&gt;[/netmask][, (allow|deny) &lt;indirizzo-ip&gt;[/netmask], ...]**<br />
Definisce per quali clients i settaggi [ConnectionsPerIP](#ConnectionsPerIP), MaxRequestSize and TimeForRequest non devono essere considerati.<br />
Se un IP è ammesso oppure non è nella lista, allora i settaggi saranno usati.<br />
Esempio: RequestLimitMask = deny 192.168.0.1

- - - -

**ServerId = &lt;userid&gt;|&lt;userid&gt;:&lt;groupid&gt;[,&lt;groupid&gt;, ...]**<br />
La userid e il groupid (o i groupid) che il server utilizzerà.
Se viene specificato solo uno userid, allora il groupid(s) sarà automaticamente cercato
in /etc/passwd e in /etc/group.<br />
Lo userid e il groupid dell'utente root non sono permessi.
Lo userid o il groupid può anche essere un nome non solo un numero.<br />
Default = 65534:65534<br />
Esempio: ServerId = www-data

- - - -

**ServerString = &lt;testo&gt;**<br />
Il testo che segue la scritta  'Server:' nel HTTP header della risposta.
Utilizza 'none' per rimuovere completamente la Server string dall'header HTTP.<br />
Default = Hiawatha v&lt;version&gt;<br />
Esempio: ServerString = myWebserver

- - - -

**SetResourceLimits = yes|no**<br />
Permetti a Hiawatha di configurare i limiti delle risorse e il numero di threads e di descrittori di file (file descriptors).<br />
Default = yes<br />
Esempio: SetResourceLimits = no

- - - -

**SocketSendTimeout = &lt;time&gt;**<br />
Configura il valore SO_SNDTIMEO per tutte le connessioni socket di tutti i clients.<br />
Usa lo 0 per disabilitare questa opzione.<br />
Default = 3<br />
Esempio: SocketSendTimeout = 10

- - - -

**SystemLogfile = &lt;percorso_completo_al_file&gt;**<br />
File di log per tutti i messaggi di systema e di errore.<br />
Default = /var/log/hiawatha/system.log<br />
Esempio: SystemLogfile = /var/log/hiawatha.sys

- - - -

**ThreadKillRate = &lt;numero&gt;**<br />
All'inizio, Hiawatha il numero di threads specificato da ThreadPoolSize.
Quando più threads fossero richiesti, Hiawatha li crea "al volo".
Nel momento in cui questi extra threads non sono più necessari,
un numero massimo &lt;numero&gt; di threads sono eliminati per 10 secondi.<br />
Default = 1<br />
Esempio: ThreadKillRate = 10

- - - -

**ThreadPoolSize = &lt;size&gt;**<br />
Dimensione iniziale del "thread pool".<br />
Default = 25<br />
Esempio: ThreadPoolSize = 50

- - - -

**Throttle = (&lt;main-mimetype&gt;/[&lt;sub-mimetype&gt;]|.&lt;extension&gt;):&lt;speed in kB/s&gt;**<br />
Controlla la velocità di upload speed di certi files.<br />
Esempio: Throttle = audio/mpeg:30<br />
Throttle = .mp:50

- - - -

**Tomahawk = &lt;numero della porta&gt;, &lt;Hash MD5 della password&gt;**<br />
I parametri rappresentano la porta e la password per il Tomahawk.<br />
Si può usare il telnet per connettesi a Tomahawk (localhost:&lt;numero della porta&gt;).<br />
Una volta connessi a Tomahawk, digitare 'help' per ulteriori informazioni.<br />
Esempio: Tomahawk = 81,41d0c72bd73afaa2c207064d81d5a<br />
(E' necessario che Hiawatha sia stato compilato con l'opzione  `-DENABLE_TOMAHAWK=on`)

- - - -

**TunnelSSH = &lt;ip-address&gt;[, &lt;ip-address&gt;, ...]**<br />
Questa opzione permette di collegarsi al demone SSH del nostro server quando la porta 22 non fosse disponibile.<br />
Il parametro di questa opzione è l'indirizzo IP della locazione da cui ci vogliamo connettere al server.<br />
Con PuTTY e WinSCP, useremo la tipologia HTTP proxy e abilitiamo l'opzione 'Consider proxying local host connections'.<br />
Utilizzeremo 'localhost' come hostname e l'hostname del nostro server come proxy hostname.<br />
Esempio: TunnelSSH = 123.45.67.89

- - - -

**UserDirectory = &lt;directory&gt;**<br />
Il nome della cartella web all'interno della home directory dell'utente. (Vedi UserWebsites per ulteriori informazioni).<br />
Default = public_html<br />
Esempio: UserDirectory = website

- - - -

**WaitForCGI = yes|no**<br />
Permette a Hiawatha di aspettare la fine dei processi CGI dopo aver ricevuto l'ultimo byte di output (via waitpid() call) oppure no (SIGCHLD settato a SIG_IGN).<br />
Default = yes<br />
Esempio: WaitForCGI = no

- - - -

**WorkDirectory = &lt;cartella&gt;** <br />
Definisce la cartella dove Hiawatha può memorizzare files temporanei di un upload o del Monitor.<br />
Da notare che Hiawatha cambierà per sicurezza la proprietà e diritti di accesso a questa cartella.<br />
Quindi NON usare cartelle già esistenti come ad esempio /tmp.<br />
Default = /var/lib/hiawatha<br />
Esempio: WorkDirectory = /tmp/hiawatha

- - - -

**WrapUserCGI = yes | no**<br />
Usa sempre il CGI-wrapper quando si devono gestire scripts CGI nei siti dell'utente (cfr UserWebsites per altre informazioni).<br />
Lo userid del proprietario del sito web sarà utilizzato.<br />
Default = no.<br />
Esempio: WrapUserCGI = yes

- - - -

#### CONFIGURAZIONE DEI BINDING (BINDING CONFIGURATION)<a id="BINDING-CONFIGURATION"></a>

Un binding è dove un client può connettersi (ad una porta su una interfaccia di rete).

- - - -

**BindingId = &lt;binding_id&gt;**<br />
Il binding ID può essere usato per agganciare un virtual host ad un binding
(vedi RequiredBinding per ulteriori informazioni).<br />
Esempio: BindingId = LAN

- - - -

**EnableAccf = yes|no**<br />
Abilita l' "HTTP accept filter". Questa opzione è disponibile solo su FreeBSD e richiede che il modulo accf_http sia caricato nel kernel.<br />
Default = no<br />
Esempio: EnableAccf = yes

- - - -

**EnableAlter = yes|no**<br />
Abilita i metodi PUT e DELETE nelle richieste HTTP per questo binding (vedi AlterList e UploadDirectory per ulteriori informazioni).<br />
Default = no<br />
Esempio: EnableAlter = yes

- - - -

**EnableTRACE = yes|no**<br />
Abilita il metodo TRACE nelle richieste HTTP per questo binding.<br />
Default = no<br />
Esempio: EnableTRACE = yes

- - - -

**Interface = &lt;indirizzo IP&gt;**<br />
L'indirizzo  IP dell'interfaccia che deve essere utilizzata.<br />
Default = 0.0.0.0 (IPv4)<br />
Esempio: Interface = 192.168.0.1

- - - -

**MaxKeepAlive = &lt;numero&gt;**<br />
Numero massimo di stay-alive dopo la prima richiesta. Dopo di che la connessione viene chiusa.<br />
Naturalmente il browser si può riconnettere. Ma questo fornisce agli altri utenti la possibilità
di connettersi in caso di un webserver particolarmente "affollato".<br />
Default = 50<br />
Esempio: MaxKeepAlive = 100

- - - -

**MaxRequestSize = &lt;size&gt;**<br />
The maximum size of a request in kilobytes the webserver is allowed to receive. This does not include PUT requests.<br />
Default = 64<br />
Esempio: MaxRequestSize = 256

- - - -

**MaxUploadSize = &lt;size&gt;**<br />
La dimensione massima in megabyte che il webserver è autorizzato a ricevere da una richiesta PUT.<br/>
Il limite massimo configurabile è 2047 megabyte.<br />
Default = 1<br />
Esempio: MaxUploadSize = 15

- - - -

**Port = &lt;port number&gt;**<br />
Il numero della porta che sarà usata dal binding. Questo è un parametro obbligatorio.<br/>
Esempio: Port = 80

- - - -

**RequiredCA = &lt;CA certificate file&gt;[, &lt;CA CRL file&gt;]**<a id="RequiredCA"></a><br />
Utilizza il certificato CA presente in questo file per autenticare gli utenti.<br />
Gli utenti privi di un certificato rcavato da uno dei CA elencati, non saranno ammessi.<br/>
Esempio: RequiredCA = /etc/ssl/cacert.pem, /etc/ssl/cacrl.pem<br />
(requires that Hiawatha was not compiled with `-DENABLE_SSL=off`)

- - - -

**SSLcertFile = &lt;SSL private key and certificate file&gt;**<a id="SSLcertFile"></a><br />
Cripta le connessioni del binding corrente con la chiave privata SSL e il certificato presenti nel file specificato.<br/>
Anche i certificati intermedi vanno a finire in questo file.<br>
Accertati che l'ordine combaci con la catena di SSL: prima i certificato dell'host e per ultimi i certificati CA.<br/>
Esempio: SSLcertFile = my_domain.pem<br />
(requires that Hiawatha was not compiled with `-DENABLE_SSL=off`)

- - - -

**TimeForRequest = [&lt;time1&gt;, ]&lt;time2&gt;**<br />
Numero massimo di secondi a disposizione di un client per inviare la sua richiesta HTTP.<br/>
Il valore *time1* è usato per la prima richiesta, mentre il *time2* per le richieste successive (Keep-Alive time).<br/>
Se il valore *time2* è omesso, *time1* è usato per tutte le richieste.<br/>
Default = 5, 30<br />
Esempio: TimeForRequest = 2, 45

- - - -

#### CONFIGURAZIONE DI UN VIRTUAL HOST

Questa è la sezione in cui si configurano i domini (virtuali) che il webserver gestirà.<br />
Il primo dominio **NON** deve essere inserito in questa sezione
in quanto essendo il dominio di default non è virtuale, ma reale.<br />
E' una buona pratica di prudenza configurare l'indirizzo IP del webserver come "Hostname" del sito di default
che ha come unica pagina una pagina vuota.<br />
In questo modo gli strumenti automatici di scansione dei siti web, non saranno in grado di trovare alcuna vulnerabilità.

- - - -

**AccessList = (allow|deny|pwd) &lt;ip-address&gt;[/netmask][, (allow|deny|pwd) &lt;ip-address&gt;[/netmask], ...]**<br />
Define which IPs have access to the website. If an IP does not match an entry in the list, access is granted. 'all' is an alias for 0.0.0.0/0.<br />
The IP address of the machine that connects and the IP address specified in the X-Forwarded-For header field (deny only) will be used to find a match.<br />
'allow' gives access, 'deny' denies access and 'pwd' gives access if a valid password has been given (see PasswordFile for more information).<br />
Esempio: AccessList = deny 10.0.0.13, allow 10.0.0.0/24, deny all

- - - -

**AccessLogfile = &lt;filename with full path&gt;[,daily|monthly|weekly]**<br />
File di log per le richieste HTTP. Hiawatha li può ruotare su base giornaliera, settimanale o mensile.<br/>
Default = /var/log/hiawatha/access.log<br />
Esempio: AccessLogfile = /var/log/hiawatha.acc, weekly

- - - -

**Alias = &lt;softlink&gt;:&lt;directory&gt;**<br />
Un alias è un softlink ad una cartella. Ogni richiesta a &lt;websiteroot&gt;/&lt;softlink&gt; sarà rediretta a &lt;directory&gt;.<br/>
Esempio: Alias = /doc:/usr/share/doc

- - - -

**AllowDotFiles = &lt;yes|no&gt;**<br/>
Permette ad Hiawatha di effettuare l'upload di files il cui nome comincia con un punto (in ambiente Unix sono i files "nascosti").
Le richieste per i files `.hiawatha` sono sempre bloccate.<br/>
Default = no<br/>
Esempio: AllowDotFiles = yes

- - - -

**AlterGroup = &lt;groupname&gt;[, &lt;groupname&gt;, ...]**<br />
Il &lt;groupname&gt; è il nome del gruppo di cui un utente deve far parte per poter usare i metodi HTTP PUT e DELETE.
(vedi PasswordFile e AlterList per ulteriori informazioni).<br />
Esempio: AlterGroup = publishers

- - - -

**AlterList = (allow|deny|pwd) &lt;ip-address&gt;[/netmask][, (allow|deny|pwd) &lt;ip-address&gt;[/netmask], ...]**<br />
Definisce quali IP hanno il permesso per poter usare i metodi HTTP PUT e DELETE. Se un IP non corrisponde ad uno degli
elementi della lista, allora l'utilizzo dei due metodi viene negato.<br />
Il termine 'all' è un alias per intendere 0.0.0.0/0.0.<br />
L'indirizzo IP della macchima che si connette e l'IP specificato nel campo intestazione (header) `X-Forwarded-For`
(solo negazione, deny only) sarà utilizzato per cercare un eventuale corrispondenza.<br />
Attenzione ai tentativi di effettuare l'upload di script CGI! Utilizza l'opzione "ExecuteCGI = no" in una sezione Directory
per disabilitarre l'esecuzione di un CGI. (vedi EnableAlter, AlterGroup and AlterMode per ulteriori informazioni).<br />
Esempio: AlterList = deny 10.0.0.13, allow 10.0.0.0/24, deny all

- - - -

**AlterMode = &lt;filemode&gt;**<br />
I file che sono creati via PUT, avranno i permessi configurati a &lt;filemode&gt; (vedi AlterList per ulteriori informazioni).<br/>
Default = 640<br/>
Esempio: AlterMode = 664

- - - -

**BanByCGI = yes|no[, &lt;max_secondi&gt;]**<br />
Permette ad un'applicazione CGI di bannare un client via l'intestazione CGI (CGI header) `X-Hiawatha-Ban: <max_secondi>`.<br/>
Il parametro &lt;max_secondi&gt; rappresenta il numero massimo di secondi di "ban" che un'applicazione CGI è autorizzata ad usare nei confronti di un client.<br/>
Default = no<br />
Esempio: BanByCGI = yes

- - - -

**CustomHeader = &lt;chiave&gt;: &lt;valore&gt;**<br/>
Configura un'intestazione HTTP personalizzata per ogni risposta.<br/>
Esempio: CustomHeader = Access-Control-Allow-Origin: *

- - - -

**DenyBody = &lt;espressione_regolare&gt;**< id="DenyBody"></a><br/>
Se il corpo della richiesta corrisponde all'espressione regolare, Hiawatha ritorna un codice "403 Forbidden".<br/>
Esempio: DenyBody = ^.\*%3Cscript.\*%3C%2Fscript%3E.\*$

- - - -

**EnablePathInfo = yes|no**<br/>
Accetta degli URL del tipo `/index.php/parametro` nel caso che `/index.php` esista e che l'estensione .php sia stata
configurata essere un programma CGI.<br/>
In questo caso '`/parametro`' sarà inserito nella variabile d'ambiente `PATH_INFO`.<br/>
Default = no<br/>
Esempio: EnablePathInfo = yes

- - - -

**EnforceFirstHostname = yes|no**<br />
Se il nome host usato nella URL non è uguale al primo ndella lista dei settaggi degli Hostname, allora Hiawatha
invierà un redirect 301 con quel nome host. Questa opzione è igorata se il primo nome host inizia con un carattere jolly.<br/>
Default = no<br/>
Esempio: EnforceFirstHostname = yes

- - - -

**ErrorHandler = &lt;error code&gt;:&lt;filename&gt;[?key=value&...]**<br/>
Quando si verificasse un errore con codice 401, 403, 404, 501 o 503, il file specificato sarà inviato al browser.<br/>
Il WebsiteRoot e l' ErrorHandler insieme, devono formare il percorso completo al file.<br/>
Il codice d'errore generato può essere ricavato alla variabile d'ambiente HTTP_GENERATED_ERROR.<br/>
Per sostituire il codice HTTP ritornato in uno script CGI, bisogna usare l'intestazione HTTP "Status" , come ad esempio "Status: 404".<br/>
Esempio: ErrorHandler = 404:/error.php?code=404

- - - -

**ErrorLogfile = &lt;filename with full path&gt;**<br/>
Nome del file di log che verrà scritto con lo stdiut dei processi CGI.<br/>
Default = /var/log/hiawatha/error.log<br/>
Esempio: ErrorLogfile = /var/log/hiawatha.err

- - - -

**ErrorXSLTfile = &lt;percorso-completo-al-file-XSLT&gt;**<br/>
In caso di errore, Hiawatha utilizza il file XSLT per generare il messaggio d'errore.<br/>
Nel caso un file non fosse definito, Hiawatha utilizzerà un messaggio d'errore standard.<br/>
Un esempio di file XML generato in caso d'errore, può essere trovato nella cartella extra/error.xml all'interno del pacchetto dei sorgenti.<br/>
Esempio: ErrorXSLTfile = /etc/hiawatha/error.xslt<br/>
(E' necessario che Hiawatha non sia stato compilato con l'opzione `-DENABLE_XSLT=off`)

- - - -

**ExecuteCGI = yes|no**<a id="ExecuteCGI"></a><br/>
Permette l'esecuzione di programmi CGI.<br/>
Default = no<br/>
Esempio: ExecuteCGI = yes

- - - -

**FileHashes = &lt;File che contiene l'hash di files&gt;**<br/>
Fornisce ad Hiawatha il percorso ad un file che contiene un hash SHA256 per ogni singolo file nella cartella radice.<br/>
Prima di servire un file, Hiawatha verifica l'hash di quel file confrontandolo con quelli presenti nel file-hash.<br/>
Se non esiste corrispondenza, l'accesso è negato. Questo sistema protegge contro la modifica di file o l'upload di malware.<br/>
Anche gli script FastCGI sono verificati nella stessa maniera se il server FastCGI può essere raggiunta via un socket UNIX.<br/>
Il file di hash può essere creato utilizzando l'opzione `-s` del comando [wigwam](/library/Manuale/04-wigwam.md).<br/>
Esempio: FileHashes = /etc/hiawatha/hashes/my_website.txt

- - - -

**FollowSymlinks = yes|no**<br/>
Permette a Hiawatha di seguire i link simbolici a file e cartella. I link simbolici che risiedono all'interno della cartella
radice o che sono proprietà dell'utente root sono sempre seguiti.<br/>
Nota bene che questo non si applica ai link simbolici CGI che sono eseguiti via FastCGI, in quanto Hiawatha non
è in grado di cercare link simbolici che risiedono su server FastCGI remoti.<br/>
Default = no<br/>
Esempio: FollowSymlinks = yes

- - - -

**Hostname = &lt;hostname&gt;, [&lt;hostname&gt;, ...]**<br/>
Nome (o nomi) dell'host che Hiawatha gestirà. E' possibile usare un carattere jolly come iniziale, ad eccezione
del primo nome host (un nome valido è richiesto in caso di un errore 301).<br/>
Il parametro Hostname è obbligatorio.<br/>
Esempio: Hostname = www.my-domain.com, *.my-domain.com, www.some-alias.com

- - - -

**IgnoreDotHiawatha = yes|no**<br/>
Specifica se Hiawatha deve ignorare o meno i files `.hiawatha`.<br/>
Default = no<br/>
Esempio: IgnoreDotHiawatha = yes

- - - -

**LoginMessage = &lt;text&gt;**<br/>
Definisce il messaggio che sarà visualizzato nella finestra di login in caso di un'autenticazione HTTP (vedi il parametro
PasswordFile per ulteriori informazioni).<br/>
Quando si usasasse l'autenticazione Digest HTTP, il LoginMessage non dovrebbe contenere il carattere ':'.<br/>
Default = Private page<br/>
Esempio: LoginMessage = Hugo's MP3 collection

- - - -

**MonitorRequests = yes|no**<br/>
Rende disponibile al Hiawatha Monitor tutte le informazioni disponibili relative all'host corrente.<br/>
Default = no<br/>
Esempio: MonitorRequests = yes

- - - -

**NoExtensionAs = &lt;extension&gt;**<br/>
Se il file richiesto non ha un'estensione, viene trattato come se la sua estensione fosse &lt;extension&gt;.<br/>
Esempio: NoExtensionAs = cgi

- - - -

**PasswordFile = ((Basic|Digest):&lt;passwordfile&gt;)|none[,&lt;groupfile&gt;]**<a id="PasswordFile"></a><br/>
Quando questa opzione viene settata, l'autenticazione HTTP viene abilitata. Utilizzare il percorso completo al file delle password
quando il file delle password fosse da utilizzare anche per delle sottocartelle.
Le password possono essere create utilizzando il programma [wigwam(1)](/library/Manuale/04-wigwam.md).
Il "realm" per l'autenticazione Digest HTTP deve essere uguale al testo configurato via LoginMessage.
Il &lt;groupfile&gt; contiene i nomi dei gruppi seguiti dai nomi degli utenti che sono membri di quel gruppo.
Il formato delle righe nel groupfile è: &lt;groupname&gt;:&lt;username&gt;[ &lt;username&gt; ...]<br/>
Esempio: PasswordFile = basic:/var/www/.passwords,/var/www/.groups

- - - -

**PreventCSRF = yes|no**<br/>
Previene una *Cross-site Request Forgery* ignorando tutti i cookie inviati da un browser quando si seguisse un link diretto all'esterno del sito corrente.
Questa configurazione potrebbe causare dei problemi per gli utenti che usassero degli strumenti per nascondere/rimuovere
la stringa dell'intestazione *Referer HTTP* durante la navigazione. Nota bene che questa protezione non è sicura al 100%.<br/>
Default = no<br/>
Esempio: PreventCSRF = yes

- - - -

**PreventSQLi = yes|no**<a id="PreventSQLi"></a><br/>
Previene le SQL-injection individuando i tentativi di iniezione e vietando la corrispondente richiesta attraverso un codice di risposta HTTP 409.<br/>
Nonostante venga fatto il massimo sforzo, è fondamentale comprendere che l'individuazione dei tentativi di SQL-injection non è garantita al 100%.<br/>
Notiamo inoltre che questa opzione potrebbe avere ripercussioni negative sull'efficienza del server web.
Da usarsi quindi con cautela.<br/>Vedi anche l'opzione [BanOnSQLi](#BanOnSQLi).<br/>
Default = no<br/>
Esempio: PreventSQLi = yes

- - - -

**PreventXSS = yes|no**<br/>
Previene il *cross-site scripting* via URL sostituendo i caratteri `<`, `>`, `"` e `'` presenti nella URL con un segno di sottolineatura `_`.<br/>
Default = no<br/>
Esempio: PreventXSS = yes

- - - -

**RequiredBinding = &lt;binding_id&gt;[, &lt;binding_id&gt;, ...]**<br/>
Di default un host virtuale può essere raggiunto attraverso un binding qualunque. Invece, utilizzando questa opzione,
è possibile specificare quali binding(s) sono utilizzabili per visitare quel host virtuale (si veda il capitolo [BINDING CONFIGURATION](#BINDING-CONFIGURATION)
per ulteriori informazioni).<br/>
Esempio: RequiredBinding = LAN

- - - -

**RandomHeader = &lt;lunghezza&gt;**<br/>
Aggiunge una intestazione HTTP X-Random alla risposta per una connessione HTTPS.<br/>
L'intestazione contiene una stringa casuale. La lunghezza della stringa è un numero a caso fra 1 e &lt;lunghezza&gt;.<br/>
Questa intestazione aiuta a prevenire il fatto che un attaccante possa indovinare quale file sia stato richiesto
basandosi sulla lunghezza della risposta. Il valore di &lt;lunghezza&gt; deve essere compreso fra 10 e 1000.<br/>
Esempio: RandomHeader = 250

- - - -

**RequiredCA = ...**<br/>
Usa questa opzione all'interno di un blocco di host virtuale se si desidera utilizzare le capacità SNI di Hiawatha.<br/>
Si veda l'opzione [RequiredCA](#RequiredCA) nel capitolo [BINDING CONFIGURATION](#BINDING-CONFIGURATION) per ulteriori informazioni.

- - - -

**RequiredGroup = &lt;groupname&gt;[, &lt;groupname&gt;, ...]**<br/>
Il &lt;groupname&gt; è il nome del gruppo a cui un utente deve appartenere per ottenere l'accesso
(si veda [PasswordFile](#PasswordFile) per ulteriori informazioni).<br/>
Esempio: RequiredGroup = webadmins,staff

- - - -

**RequireSSL = yes|no[,HSTS time]**<br/>
Specifica che un dominio deve essere visitato attraverso una connessione SSL.
Nel caso sia visitato via una semplice connessione HTTP, Hiawatha effettuerà un redirect (via un codice 301) verso un
URL HTTPS.<br/>
L' HSTS time rappresenta il valore di max-age dell'intestazione HTTP *Strict-Transport-Security*.<br/>
Default = no,31536000<br/>
Esempio: RequireSSL = yes,2678400<br/>
(E' necessario che Hiawatha non sia stato compilato con l'opzione `-DENABLE_SSL=off`)

- - - -

**ReverseProxy [!]&lt;pattern&gt; http[s]://&lt;hostname&gt;[:&lt;port&gt;][/&lt;path&gt;] [&lt;timeout&gt;] [keep-alive]**<br/>
Inoltra la richiesta con una URL che corrisponde all'espressione regolare &lt;pattern&gt; ad un altro server web.
In questo caso &lt;path&gt; è messo davanti alla URL originale.<br/>
Si noti che la selezione del *reverse proxy* viene prima della gestione dell' *URL toolkit*.
Qualora &lt;hostname&gt; fosse un indirizzo IP, il valore dell'intestazione HTTP *Host* rimane invariato.
In caso contrario, viene sostituita con il valore di &lt;hostname&gt;.<br/>
La connessione viene chiusa dopo &lt;timeout&gt; secondi, che per default è fissato a 5 secondi.<br/>
Di default, Hiawatha non utilizza connessioni *keep-alive* verso il server web finale. Si può abilitare questo comportamento
aggiungendo `keep-alive` alla riga della configurazione.<br/>
Esempio: ReverseProxy ^/icons http://resources.lan/images

- - - -

**RunOnAlter = &lt;path to program&gt;**<br/>
Esegue un programma dopo che un client ha inviato una richiesta PUT o DELETE. Informazioni riguardo alla richiesta
sono poi messe in variabili d'ambiente, proprio come gli script CGI.<br/>
Esempio: RunOnAlter = /usr/local/sbin/alter-script

- - - -

**Setenv &lt;key&gt; = &lt;value&gt;**<br/>
Definisce le opzioni d'ambiente per i programmi CGI.<br/>
Esempio: Setenv PHPRC = /var/www/conf

- - - -

**ScriptAlias = &lt;softlink&gt;:&lt;script&gt;**<br/>
Uno *script alias* è un softlink virtuale verso uno script CGI.
Ogni richiesta verso &lt;websiteroot&gt;/&lt;softlink&gt; sarà reindirizzata verso &lt;script&gt;.<br/>
Esempio: ScriptAlias = /script.cgi:/usr/lib/script.cgi

- - - -

**ShowIndex = yes|no|&lt;percorso-completo-ad-un-file-XSLT&gt;|xml**<br/>
Ritorna il listato di una cartella in formato HTML nel caso un file index non esistesse.<br/>
Se si desidera cambiare completamento il layout, bisogna specificare il percorso ad un file XSLT.<br/>
Se il file XSLT non fosse disponibile oppure fosse stata impostata l'opzione `xml`, Hiawatha visualizzerà
l'XML della cartella.<br/>
Un esempio di output XML è reperibile in  *extra/index.xml* all'interno del pacchetto dei sorgenti.

Default = no<br/>
Esempio: ShowIndex = /etc/hiawatha/index.xslt

(E' necessario che Hiawatha non sia stato compilato con l'opzione `-DENABLE_XSLT=off`)

- - - -

**SSLcertFile = ...**<br/>
Si utilizzi questa opzione in un blocco di un virtualhost se si vuol far uso delle capacità SNI di Hiawatha.<br/>
Si veda l'opzione [SSLcertFile](#SSLcertFile) nel capitolo [BINDING CONFIGURATION](#BINDING-CONFIGURATION) per ulteriori informazioni.

- - - -

**StartFile = &lt;filename&gt;**<br/>
Indica il nome di file che deve essere inviato al browser quando venga richiesta una cartella.<br/>
Default = index.html<br/>
Esempio: StartFile = start.php

- - - -

**TimeForCGI = &lt;secondi&gt;**<br/>
Tempo massimo in secondi, concesso ad un processo CGI per completare il proprio lavoro.<br/>
Default = 5<br/>
Esempio: TimeForCGI = 15

- - - -

**TriggerOnCGIstatus = yes|no**<br/>
Visualizza un messaggio d'errore HTTP o invoca un ErrorHandler quando un processo CGI emette
una riga con un'intestazione con uno Status HTTP.<br/>
Default = no<br/>
Esempio: TriggerOnCGIstatus = yes

- - - -

**UserWebsites = yes|no**<br/>
Attiva i website utente per questo host (virtuale). Si tratta della URL `/~user/`.<br/>
Default = no<br/>
Esempio: UserWebsites = yes

- - - -

**UseFastCGI = &lt;fcgi_server_id&gt;[, &lt;fcgi_server_id&gt;, ...]**<br/>
Definisce il server FastCGI da utilizzare con l'host virtuale che si sta configurando.<br/>
Il primo server FastCGI che corrisponde (compresa l'estensione), sarà quello usato (vedi [ONFIGURAZIONE FASTCGI](#CONFIGURAZIONE-FASTCGI)
per ulteriori informazioni).<br/>
Questa opzione setta automaticamente il parametro [ExecuteCGI](#ExecuteCGI) su `yes` per il virtual host.<br/>
Esempio: UseFastCGI = PHP5

- - - -

**UseGZfile = yes|no**<br/>
Se disponibile, a fronte della richiesta di un &lt;file-richiesto&gt;, verrà restituito il corrispondente
&lt;file-richiesto&gt;.gz .<br/>
Default = no<br/>
Esempio: UseGZfile = yes

(opzione obsoleta?)

- - - -

**UseToolkit = &lt;toolkit_id&gt;[, &lt;toolkit_id&gt;, ...]**<a id="UseToolkit"></a><br/>
Esegue delle operazioni speciali sulla URL, come ad esempio un rewriting attraverso un espressione regolare (si veda
il capitolo [URL TOOLKIT](#URL-TOOLKIT) per ulteriori informazioni).<br/>
Esempio: UseToolkit = my_toolkit

(Si richiede che Hiawatha non sia stato compilato con l'opzione  `-DENABLE_TOOLKIT=off`)

- - - -

**UseXSLT = yes|no**<br/>
Attiva le trasformazioni XSL (si veda il capitolo [XSLT](#XSLT) per ulteriori informazioni).<br/>
Default = no<br/>
Esempio: UseXSLT = yes

(Si richiede che Hiawatha non sia stato compilato con l'opzione `-DENABLE_XSLT=off`)

- - - -

**VolatileObject = &lt;percorso-completo-ad-un-file&gt;**<br/>
Il file indicato, sarà completamente caricato in memoria prima di essere inviato al browser.
A causa di questo, il file non potrà essere più grande di 1MB.<br/>
Si usi questa opzione per files che cambiano rapidamente, come ad esempio le immagini da webcam.<br/>
Esempio: VolatileObject = /var/www/webcam.gif

- - - -

**WebDAVapp = &lt;yes|no&gt;**<br/>
Abilita il support per le applicazioni WebDAV.<br/>
Default: WebDAVapp = no<br/>
Esempio: WebDAVapp = yes

- - - -

**WebsiteRoot = &lt;directory&gt;**<br/>
Specifica la home directory per l'host virtuale.<br/>
Esempio: WebsiteRoot = /home/webmaster/website

- - - -

**WrapCGI = &lt;wrap_id&gt;**<br />
Specifica l'ID di un CGI-wrapper per questo host virtuale (vedi [cgi-wrapper(1)](/Manuale/02-cgi-wrapper.md) ulteriori informazioni).<br/>
Esempio: WrapCGI = test

- - - - -

#### CONFIGURAZIONE DI UNA CARTELLA

Questo capitolo spiega quali opzioni possono essere configurate per una specifica cartella.<br/>
Queste opzioni avranno la precedenza sulle configurazioni dell'host (virtuale).

- - - -

**Path = &lt;path|sub-path&gt;**<br/>
Il percorso (path) per arrivare ad una cartella. La presenza dell'opzione *Path* è obbligatoria.<br/>
Si noti che solo il la prima *Directory* che abbia una corrispondenza con *Path* sarà utilizzata.<br/>
Se *Path* termina con una barra (/), Hiawatha cercherà ovunque nel percorso indicato un file richiesto per una corrispondenza.<br/>
Se il *Path* non ternasse con una barra, Hiawatha comincerà a cercare una corrispondenza dall'inizio del percorso.<br/>
Esempio: Path = /var/www/cgi-bin -oppure- Path = /public_html/

- - - -

**RunOnDownload = &lt;percorso-al-programma&gt;**<br/>
Esegue un programma quando un client richiede una risorsa statica. Questo non include programmi CGI.<br/>
Le informazioni riguardo la richiesta si trovano nelle variabili d'ambiente, proprio come i CGI.<br/>
Esempio: RunOnDownload = /var/www/log_download

- - - -

**UploadSpeed = &lt;velocità&gt;,&lt;numero-massimo-di-connessioni&gt;**<br/>
Definisce la velocità di upload in kB/s per tutti i file presenti nella cartella indipendentemente dall'estensione
o dal *mimetype*. La velocità di upload per connessione sarà divisa per il numero di connessioni.<br/>
Esempio: UploadSpeed = 20,4

AccessList ,
AlterGroup ,
AlterList ,
AlterMode ,
ExecuteCGI ,
WrapCGI ,
FollowSymlinks ,
PasswordFile ,
RequiredGroup ,
Setenv ,
ShowIndex ,
StartFile ,
TimeForCGI and
UseGZfile

- - - - -

#### CONFIGURAZIONE FASTCGI<a id="CONFIGURAZIONE-FASTCGI"></a>


Questo capitolo spiega come usare uno o più server FastCGI.

- - - -

**ConnectTo = &lt;ip-address&gt;:&lt;port number&gt;|&lt;path&gt;[, &lt;ip-address&gt;:&lt;port number&gt;|&lt;path&gt;, ...]**<a id="ConnectTo"></a><br/>
Definisce l'indirizzo IP e la porta TCP oppure il socket UNIX a cui Hiawatha si deve connettere per
raggiungere il server FastCGI.<br/>
Esempi:<br/>
ConnectTo = 127.0.0.1:2004 (IPv4)<br/>
ConnectTo = [::1]:2004 / ::1.2004 (IPv6)<br/>
ConnectTo = /tmp/hiawatha.sock (UNIX socket)

- - - -

**Extension = &lt;estensione&gt;[, &lt;estensione&gt;, ...]**<br/>
L'estensione dello script che il server FastCGI è in grado di interpretare. Se nessuna estensione viene specificata,
tutte le richieste saranno inviate al server FastCGI.<br/>
Esempio: Extension = php

- - - -

**FastCGIid = &lt;fcgi_server_id&gt;**<br/>
Associa ad ogni server FastCGI un identificativo unico. Si utilizzi questo identificativo nella configurazione
del FastCGI nel contesto di un host vistuale.<br/>
Esempio: FastCGIid = PHP5

- - - -

**ServerRoot = &lt;percorso&gt;**<br/>
Se il server FastCGI viene eseguito in un ambiente chroot, si utilizzi questo settaggio per specificare la cartella chroot.<br/>
Esempio: ServerRoot = /var/www/chroot

- - - -

**SessionTimeout = &lt;tempo-in-minuti&gt;**<br/>
Definisce la massima durata di una sessione CGI per questo server FastCGI. Sarà usata solamente quando si specifichino
multipli *[ConnectTo](#ConnectTo)*.<br/>
Default = 15<br/>
Esempio: SessionTimeout = 30

- - - - -

#### URL TOOLKIT<a id="URL-TOOLKIT"></a>

In questo capitolo viene spiegato come utilizzare gli *URL toolkit*. Per utilizzare gli *URL toolkit*
Hiawatha non deve essere stato compilato con l'opzione `-DENABLE_TOOLKIT=off`.<br/>
I principali comandi utilizzabili in un toolit sono i seguenti:

- - - -

**Call &lt;toolkit_id&gt;**<br/>
Esegue la sezione del toolkit &lt;toolkit_id&gt; e poi continua con la sezione corrente.<br/>
Esempio: Call other_rule_set

- - - -

**Header &lt;chiave&gt; [!]&lt;pattern&gt; &lt;azione&gt;**<br/>
Esegue un'azione quando l'intestazione HTTP &lt;key&gt; corrisponde all'espressione regolare &lt;pattern&gt;,
dove &lt;azione&gt; può essere una delle seguenti voci:<br/>
*Call*, *DenyAccess*, *Exit*, *Goto*, *Redirect*, *Return*, *Skip* e *Use*.<br/>
Un pattern negativo (cioé con davanti un punto esclamativo) non può essere usato con l'azione di *Redirect*.

- - - -

**Match [!]&lt;pattern&gt; &lt;azione&gt;**<br/>
Esegue l'azione quando l'URL corrisponde all'espressione regolare &lt;pattern&gt;,
dove &lt;azione&gt; può essere una delle seguenti:<br/>
*Ban*, *Call*, *DenyAccess*, *Exit*, *Expire*, *Goto*, *Redirect*, *Return*, *Rewrite*, *Skip*, *UseFastCGI*.<br/>
Si utilizzi *MatchCI* per eseguire una regex che non tenga conto di MAIUSCOLE/minuscole.
Un *pattern* negato (cioé con davanti un punto esclamativo) non può essere usato con le azioni di *redirect* e *rewrite*.

- - - -

**RequestURI exists|isfile|isdir Return|Exit**<br/>
Se la URL richiesta esiste sul server allora non continuare con questo URL toolkit.<br/>
Esempio: RequestURI isfile Return

- - - -

**ToolkitId = &lt;toolkit_id&gt;**<a id="ToolkitId"></a><br/>
Il toolkit ID può essere usato per collegate le regole del toolkit con un host virtuale.
Si veda anche l'opzione [UseToolkit](#UseToolkit).<br/>
Esempio: ToolkitId = my_toolkit

- - - -

**UseSSL &lt;azione&gt;**<br/>
Esegue una certa azione quando il client si connetta attraverso una connessione sicura SSL, dove  &lt;azione&gt;
può essere una delle seguenti:<br/>
*Call*, *Exit*, *Goto*, *Return*, *Skip*.

**Un punto esclamativo posto di fronte ad un pattern (*negative pattern matching*) fa si che Hiawatha esegua
l'azione quando il pattern non corrisponda. Le istruzioni &lt;azione&gt; menzionate sopra sono descritte qui:**


**Ban &lt;secondi&gt;**</br/>
Esegue il *ban* (impedisce ogni tentativo di accesso) del client per &lt;secondi&gt; secondi.


- - - -

**DenyAccess**<br/>
Nega l'accesso al file richiesto (riportando un errore 403) e termina l'esecuzione del toolkit.

- - - -

**Exit**<br/>
Termina l'esecuzione del toolkit.

- - - -

**Expire &lt;time&gt; seconds|minutes|hours|days|weeks|months [public|private] [Exit|Return]**<br/>
Aggiunge un'intestazione HTTP `Expires` con valore pari al *timestamp* corrente +  &lt;time&gt;.<br/>
L'opzione *public/private* (il default è *private*) definisce il valore dell'intestazione `Cache-Control`<br/>
Il comportamento di default per Hiawatha è quello di continuare dopo un azione *Expire*.

- - - -

**Goto &lt;toolkit_id&gt;**<br/>
Esegue &lt;toolkit_id&gt; e termina l'esecuzione del toolkit corrente.

- - - -

**Redirect &lt;url&gt;**<br/>
Ridirige, con un codice 301, il browser verso l'URL specificato e termina l'esecuzione del toolkit.

- - - -

**Return**<br/>
Ritorna dalla sezione corrente dell'UrlToolkit.

- - - -

**Rewrite &lt;replacement&gt; [&lt;max_loop&gt;] [Continue|Return]**<br/>
Riscrive l'URL corrente usando &lt;replacement&gt;.<br/>
Esempi:<br/>
`Match ^/pics/(.*) Rewrite /images/$1` sostituirà `/pics/logo.gif` con `/images/logo.gif`.<br/>
`Match a Rewrite b 3` sostituirà `/aaaaa.html` con `/bbbaa.html`.

Il valore di default di &lt;max_loop&gt; è 1, mentre il massimo è 20.

L'esecuzione di *Rewrite* terminerà il processamento del toolkit a meno che non siano state impostate le opzioni
*Continue* oppure *Return*.

- - - -

**Skip &lt;numero&gt;**<br/>
Salta le seguenti &lt;numero&gt; linee ([ToolkitId](#ToolkitId) escluso).<br/>
Esempio: Skip 2

- - - -

**Use &lt;url&gt;**<br/>
Sostituisce l'URL corrente con &lt;url&gt; e termina il processamento del toolkit.

- - - -

**UseFastCGI &lt;fcgi_id&gt;**<br/>
Utilizza un server FastCGI con identificativo &lt;fcgi_id&gt; e termina il processamento del toolkit.

**L'URL originale è memorizzato nella variabile d'ambiente SCRIPT_URL. Prima di usare le regole di un URL toolkit,
 si utilizzi il programma *['wigwam'](/Manuale/04-wigwam.md)* per verificare i risultati delle vostre regole.**


Esempio:
```
    VirtualHost {
           ...
           UseToolkit = my_toolkit
    }

    UrlToolkit {
           ToolkitId = fix_PHP
           Match ^/index.php4(.*) DenyAccess
           Match ^/index.php5(.*) Rewrite /index.php$1
    }

    UrlToolkit {
           ToolkitId = my_toolkit
           Call fix_PHP
           RequestURI isfile Return
           Match ^/(.*) Rewrite /index.php?page=$1
    }
```

- - - - -

#### XSLT<a id="XSLT"></a>

Se richiediamo un file XML, Hiawatha può eseguire una trasformazione XSL nel caso un foglio XSLT sia presente.
Per un certo file XML (<nome>.xml) richiesto, nella directory corrente deve esistere un file  '<nome>.xslt'
oppure 'index.xslt'  o in alternativa, nella WebsiteRoot, deve esserci un file 'index.xslt'.
In caso contrario (non ci sono files .xslt corretti), verrà restituito il file XML stesso senza modificazioni.
Le variabili d'ambiente che sono disponibili durante l'esecuzione CGI  sono disponibli come parametri XSLT.
Le variabili passate sulla URL hanno nome che inizia con  'GET_',
le variabili POST iniziano con 'POST_' e i cookies iniziano con 'COOKIE_'.

- - - - -

#### CGI OUTPUT CACHE

Hiawatha può mantenere una cache dell'risultato di una applicazione CGI. Se e per quanto tempo questa cache debba essere
mantenuta, dipende dall'applicazione stessa.<br/>
L'applicazione può utilizzare le seguenti intestazioni CGI per controllare il comportamento della cache riguardo
al proprio output.<br/>
Per poter utilizzare questa caratteristica, è necessario che Hiawatha non sia stato compilato con l'opzione `-DENABLE_CACHE=off`.<br/>

- - - - -

**X-Hiawatha-Cache: &lt;secondi&gt;**&gt;<br/>
Il risultato, ovvero l'output del programma può essere mantenuto in cache per &lt;secondi&gt secondi. Il valore minimo
è 2 mentre il massimo è 3600 (un'ora).<br/>
Esempio: X-Hiawatha-Cache: 600
    
- - - - -

**X-Hiawatha-Cache-Remove: &lt;url&gt;**<br/>
L'output di &lt;url&gt; deve essere rimosso dalla cache.
Si utilizzi questa intestazione quando si sia aggiornata una pagina in cache del proprio CMS.<br/>
Si utilizzi il termine `all` come URL per eliminare la cache relativa a tutto il sito web corrente.<br/>
Esempio: X-Hiawatha-Cache-Remove: /about

- - - - -

#### SETTAGGI UTENTE PER CARTELLA

Un utente può sovrascrivere, per una certa cartella, i settaggi indicati qui sotto:<br>

AccessList ,<br/>
AlterGroup ,<br/>
AlterList ,<br/>
AlterMode ,<br/>
ErrorHandler ,<br/>
LoginMessage ,<br/>
PasswordFile ,<br/>
RequiredGroup ,<br/>
Setenv ,<br/>
ShowIndex ,<br/>
StartFile and<br/>
UseToolkit (valido solo nella cartella radice di un sito)

Questo può essere fatto inserendo uno o più di questi settaggi in un file `.hiawatha` dentro a quella cartella. Hiawatha non cercherà alcun `.hiawatha` nella cartella root del disco.

- - - -

#### MIMETYPES


Definisce i mimetypes dei file in `/etc/hiawatha/mimetypes.conf`.

&lt;mimetype&gt; &lt;extension&gt; [&lt;extension&gt; ...]<br />
Esempio: image/jpeg jpg jpeg jpe


- - - -

#### SIGNALS

**TERM**<br />
Spegne il webserver.

**HUP**<br />
Chiude tutti i file di log rimasti aperti.

**USR1**<br />
Riabilita tutti gli IP bannati.

**USR2**<br />
Svuota la cache interna (è necessario che Hiawatha non sia stato compilato con l'opzione `-DENABLE_CACHE=off`).

- - - -

#### FILES

/usr/sbin/hiawatha<br />
/etc/hiawatha/hiawatha.conf<br />
/etc/hiawatha/mime.types<br />
/etc/hiawatha/cgi-wrapper.conf<br />

- - - -

#### VEDI ANCHE


[cgi-wrapper(1)](/Manuale/02-cgi-wrapper.md), [wigwam(1)](/Manuale/04-wigwam.md)

- - - -

#### AUTORE

Hugo Leisink <hugo@hiawatha-webserver.org> - [http://www.hiawatha-webserver.org/](http://www.hiawatha-webserver.org/)

Pagina originale: [https://www.hiawatha-webserver.org/manpages/hiawatha](https://www.hiawatha-webserver.org/manpages/hiawatha)