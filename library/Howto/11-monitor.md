# Monitor

Questa pagina descrive come usare la funzionalità di monitoraggio di Hiawatha.<br/>
Per usare la funzionalità di monitoraggio, abbiamo bisogno di due componenti:<br/>
il webserver Hiawatha e lo Hiawatha Monitor, che è una applicazione web basata su PHP/MySQL.<br/>
Abbiamo bisogno di uno o più server che stiano eseguendo il webserver Hiawatha (i clients da monitorare)
e di un server su cui eseguire il sito web dello Hiawatha Monitor (il server monitorante).<br/>
Naturalmente possiamo usare un server sia come client monitorato che come server monitorante.

### Compilazione di Hiawatha

Se decidiamo di usare la funzionalità di monitoraggio, assicuriamoci che l'istanza di Hiawatha da monitorare
sia stata compilata con **-DENABLE_MONITOR=on** e **-DENABLE_XSLT=on**.

****

### Configurazione dei clients da monitorare

Per attivare la funzionalità di monitoraggio del Hiawatha webserver, usiamo il settaggio **MonitorServer**.

    MonitorServer = <IP address of the monitor server>

L'indirizzo IP del server monitorante sarà usata dal webserver Hiawatha
per creare un lista di accesso che servirà a proteggere i files di log
oggetto del monitoraggio.<br/>Solo al server monitorante sarà concesso di scaricare i files di log.

****

### Installazione del webserver di monitoraggio

[Scarichiamo](https://www.hiawatha-webserver.org/download) il pacchetto del sito di monitoraggio e scompattiamolo in una cartella adatta. Seguiamo quindi le istruzioni incluse nel file README.<br/>
Il sito che si occuperà del monitoraggio necessita del PHP5 del MySQL e del cron daemon.<br/>
Avremo bisogno inoltre di una conoscenza anche solo basica di Hiawatha per installare correttamente il sito di monitoraggio.

****

### Configurazione del sito di monitoraggio

Facciamo puntare il nostro browser sul sito di monitoraggio, facciamo il login con username
'admin' e password 'monitor' e andiamo sul menu del CMS.<br/>
Clicchiamo su 'Webservers' e aggiungiamo i clients da monitorare.<br/>
Non dimentichiamo di cambiare la password dell'account di admin!

Pagina originale: [https://www.hiawatha-webserver.org/howto/monitor](https://www.hiawatha-webserver.org/howto/monitor)