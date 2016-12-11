# Utilizzare CGI e FastCGI

### CGI normale

Far eseguire ad Hiawatha un'applicazione CGI è piuttosto facile. Possiamo permettere l'esecuzione CGI per singolo virtual host attraverso l'opzione **ExecuteCGI**.

```
VirtualHost {
    ...
    ExecuteCGI = yes
}
```

Tutte le applicazione CGI possono essere divise in due gruppi: script CGI e programmi CGI.

****

##### Script CGI

Gli script CGI hanno bisogno di un interprete per essere eseguiti. Gli script CGI non sono altro che files di testo che non richiedono il flag di "eseguibile".  Per eseguire uno script CGI dalla linea di comando, useremo il nome del file dello script come parametro per l'interprete.

```
/path/to/interpreter script.ext
```

Se volessimo usare uno script CGI nel nostro sito web, dobbiamo indicare a Hiawatha dove poter trovare il file binario che dovrà gestire lo script. Per la gestione CGI, dovremo anche dire a Hiawatha quali tipi di files possono essere gestiti dai vari interpreti, elencandone le estensioni.


```
CGIhandler = /usr/bin/php5-cgi:php,php5
CGIhandler = /usr/bin/perl:pl
CGIhandler = /usr/bin/python:py
```

****

##### Programmi CGI

I programmi CGI possono essere eseguiti per conto loro e sono per la maggior parte dei programmi compilati in C o sono script che cominciano con una riga tipo  "#!/path/to/interpreter". I programmi CGI necessitano del flag di "esecuzione". Per eseguire un programma CGI dalla lina di comando, useremo direttamete il suo filename.

```
./program.ext
```

Per utilizzare un programma CGI nel nostro sito, Hiawatha ha bisogno di sapere l'estensione del programma CGI. Questo può essere fatto attraverso l'opzione CGIextension.

```
CGIextension = cgi
```

****

##### Sicurezza CGI

E' possibile limitare il tempo massimo di esecuzione dei programmi CGI (in secondi) utilizzando l'opzione TimeForCGI.

```
VirtualHost {
    ...
    TimeForCGI = 10
}
```

****

### FastCGI

Ogni volta che viene fatta la richiesta per un applicazione CGI, il webserver ha bisogno di eseguire l'applicazione CGI. Ma avviare e inizializzare un'applicazione prende comunque un certo tempo. Per fare in modo che il processo di eseguire un'applicazione CGI risulti più veloce, è stato inventato FastCGI. Un'applicazione FastCGI si avvia una volta sola per poi rimanere in memoria. In questo modo può rispondere a molte richieste senza l'appesantimento dell'avvio.

Ci sono due tipi di applicazioni FastCGI. Il primo tipo di applicazione viene eseguito come un daemon e rimane in ascolto su una certa porta in attesa delle richieste in arrivo dal webserver. Il secondo tipo è invece avviato dal webserver e comunica col webserver attraverso delle 'pipes'. Hiawatha supporta solo il primo tipo.

Ogni volta che viene fatta la richiesta per un applicazione CGI, il webserver ha bisogno di eseguire l'applicazione CGI. Ma avviare e inizializzare un'applicazione prende comunque un certo tempo. Per fare in modo che il processo di eseguire un'applicazione CGI risulti più veloce, è stato inventato FastCGI. Un'applicazione FastCGI si avvia una volta sola per poi rimanere in memoria. In questo modo può rispondere a molte richieste senza l'appesantimento dell'avvio.

Ci sono due tipi di applicazioni FastCGI. Il primo tipo di applicazione viene eseguito come un daemon e rimane in ascolto su una certa porta in attesa delle richieste in arrivo dal webserver.

Nell'esempio qui sotto, useremo il PHP via FastCGI. Per prima cosa, installiamo php-fpm. Le opzioni di configurazione per il php-fpm sono spiegate [qui](http://www.php.net/manual/en/install.fpm.configuration.php).
Una possibile configurazione del php-fpm per l'uso con Hiawatha è questo:

```
[www]
user = www-data
group = www-data
listen = /var/lib/hiawatha/php-fcgi.sock
pm = static
pm.max_children = 3
chdir = /
```

Notate comunque che questo è solo un esempio di configurazione. Sta a noi usare dei valori che siano ottimali per il nostro particolare sistema e situazione.

Nel file hiawatha.conf, noi dobbiamo dire a Hiawatha come raggiungere questo demone FastCGI e quando usarlo (per quali estensioni di file). Gli daremo un ID univoco e useremo questo ID con l'opzione UseFastCGI per ogni host virtuale che dovrà usare quel FastCGI server.

```
FastCGIserver {
    FastCGIid = PHP5
    ConnectTo = /var/lib/hiawatha/php-fcgi.sock
    Extension = php
}
```

```
VirtualHost {
    ...
    UseFastCGI = PHP5
}
```

Una sezione FastCGIserver con la stessa estensione di un CGIhandler, sostituisce il CGIhandler.

****

##### Bilanciare il carico su FastCGI

Se specifichiamo più di un parametro per ConnectTo, ad ogni richiesta, Hiawatha passerà da un server FastCGI all'altro. Possiamo usare questa caratteristica di bilanciamento di carico per siti web molto trafficati. Per evitare che le sessioni CGI possano venire interrotte, Hiawatha deve sapere qual'è il timeout (in minuti) di tale sessione.

```
FastCGIserver {
    FastCGIid = PHP5
    ConnectTo = 10.0.0.100:2005, 10.0.0.101:2005, 10.0.0.102:2005
    Extension = php
    SessionTimeout = 15
}
```

Assicuriamoci che ogni server FastCGI abbia lo stesso codice dell'applicazione web nella stessa cartella così come configurato in Hiawatha.

****


##### Come scrivere applicazioni FastCGI

Se non vogliamo usare il PHP, allora ci dobbiamo scrivere per conto nostro una applicazione web abilitata FastCGI.
Possiamo provare ad usare uno degli esempi seguenti come punto di partenza. Nota bne che tutti quanti richiedono il [kit di sviluppo FastCGI](http://www.fastcgi.com/drupal/node/5).

-    [C](https://www.hiawatha-webserver.org/files/fastcgi/fastcgi.c.txt)
-    [Perl](https://www.hiawatha-webserver.org/files/fastcgi/fastcgi.pl.txt)
-    [Python](https://www.hiawatha-webserver.org/files/fastcgi/fastcgi.py.txt)
-    [Ruby](https://www.hiawatha-webserver.org/files/fastcgi/fastcgi.rb.txt)

Lo strumento [cgi-fcgi](https://www.hiawatha-webserver.org/files/fastcgi/cgi-fcgi.c.txt) ([doc](http://www.fastcgi.com/devkit/doc/fcgi-devel-kit.htm#S4.2)) può essere usato per convertire un applicazione non-daemon (via pipes) in una application FastCGI daemon:

```
cgi-fcgi -start -connect :2005 /path/to/your/fastcgi/program
```

Pagina originale: [https://www.hiawatha-webserver.org/howto/cgi_and_fastcgi](https://www.hiawatha-webserver.org/howto/cgi_and_fastcgi)