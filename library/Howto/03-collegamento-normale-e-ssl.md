# Collegamento normale e SSL


### Collegamenti (bindings)

Un collegamento (binding) è dove un client si può connettere (una porta su una interfaccia).
Quasi tutti i webserver usano la porta 80 per le connessioni HTTP e la porta 443 per le connessioni
HTTPS (HTTP criptato via SSL). Come usare l'SSL è spiegato nel paragrafo seguente.
Per prima cosa, ci concentreremo sul creare un binding 'normale'. I Bindings sono creati usando la sezione **Binding**:

```
Binding {
  Port = 80
}
```

Quest'istruzione fa in modo che Hiawatha si metta in ascolto sulla porta 80 di qualunque interfaccia disponibile. Se volessimo che Hiawatha rimanga in ascolto solo su una specifica interfaccia, lo specificheremo via l'opzione Interface. Useremo l'indirizzo IP dell'interfaccia sulla quale vogliamo che Hiawatha rimanga in ascolto.

```
Binding {
  ...
  Interface = 192.168.0.1
}
```

Per evitare che un client mantenga una connessione attiva per troppo tempo, possiamo settare un timeout attraverso l'opzione **TimeForRequest**.
RequestTimeout può prendere un solo parametro, che è il timeout per ogni richiesta, oppure due parametri separati da una virgola, dove il primo parametro è il timeout per la prima richiesta mentre il secondo parametro è il timeout per tutte le richieste successive alla prima per quella connessione. Il timeout è espresso in secondi.

Un'altra opzione per proteggere il nostro webserver è l'opzione **MaxRequestSize**. Attraverso questa opzione possiamo limitare la dimensionedi una richiesta inviata da un client. Una richiesta usa memoria. L'invio di richieste molto grandi può essere usata per effettuare un DoS al nostro server, cosa che noi vorremmo evitare. La dimensione della richiesta è espressa in kilobytes.

```
Binding {
  ...
  TimeForRequest = 5, 30
  MaxRequestSize = 512
}
```

****

### Connessioni SSL

La prima cosa di cui abbiamo bisogno prima di poter usare l'SSL, è un certificato SSL X.509. Ne possiamo ottenere uno da una Certificate Authority (CA), come ad esempio Thawte o Comodo, oppure ne possiamo creare uno per conto nostro usando OpenSSL:

```
openssl genrsa -out serverkey.pem 2048
openssl req -new -x509 -days 3650 -key serverkey.pem -out server.crt
echo "" >> serverkey.pem
cat server.crt >> serverkey.pem
echo "" >> serverkey.pem
rm -f server.crt
```

A questo punto dovremmo avere un file **serverkey.pem**. Spostiamo questo file nella nostra cartella di configurazione di Hiawatha (probabilmente in /etc/hiawatha oppure in  /usr/local/etc/hiawatha) e assicuriamoci che sia leggibile solo da root (file mode 400). Infine configuriamo Hiawatha in modo che usi questo certificato per le connessioni HTTPS.

```
Binding {
  Port = 443
  SSLcertFile = /etc/hiawatha/serverkey.pem
}
```

L'ordine degli oggetti nel file serverkey.pem è importante. L'ordine è il seguente:

```
-----BEGIN RSA PRIVATE KEY-----
[webserver private key]
-----END RSA PRIVATE KEY-----

-----BEGIN CERTIFICATE-----
[webserver certificate]
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
[optional intermediate CA certificate]
-----END CERTIFICATE-----
```


Se desideriamo che certi siti web siano visitabili solo via HTTPS, possiamo forzare gli utenti ad usare l'HTTPS mettendo a 'yes' l'opzione **RequireSSL**.

```
VirtualHost {
  ...
  RequireSSL = yes
}
```

****

### Server Name Indication

Hiawatha ha il supporto per il SNI, che ci permette di gestire diversi siti web in SSL usando un solo indirizzo IP.
E' sufficiente configurare una connessione SSL come spiegato in precedenza. Per ogni virtual host che dispone di un suo certificato SSL,
semplicemente userà l'opzione **SSLcertFile** contenuto nel blocco di configurazione del virtual host.
Il certificato specificato atrraverso il blocco Binding{} è utilizzato invece quando viene richiesto un sito web che non ne ha uno proprio definito nella sua configurazione.

```
Binding {
  Port = 443
  SSLcertFile = certificate.pem
}

VirtualHost {
  Hostname = www.website.org
  ...
  SSLcertFile = website.pem
}
```

Pagina originale: [https://www.hiawatha-webserver.org/howto/bindings](https://www.hiawatha-webserver.org/howto/bindings)