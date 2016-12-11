# URL Toolkit

L' URL toolkit è un insieme di strumenti che servono ad eseguire delle azioni basate sull'URL o delle azioni sull'URL stesso.<br/>
[Espressioni regolari](http://www.regular-expressions.info/) ('regex') sono utilizzate per trovare un matching URL, un URL accoppiabile.<br/>
Possibili azioni in caso di matching URL, sono il rewriting dell'URL, la negazione dell'accesso o il reindirizzamento ad un altro URL.

La funzionalità più importante dell'URL toolkit è l'URL rewriting.<br/>
Attraverso l'URL rewriting, l'URL può essere cambiato in un altro URL prima che Hiawatha lo gestisca.<br/>
L'esempio seguente lascia gli URL che corrispondano ('matching') ad un file esistente o ad un files nella cartella images invariati,
nega l'accesso ai files presenti nella cartella data e riscrive ('rewrite') tutti gli altri URL in /index.php.


```
UrlToolkit {
    ToolkitID = rewrite
    RequestURI exists Return
    Match ^/images Return
    Match ^/data DenyAccess
    Match /(.*) Rewrite /index.php?page=$1
}

VirtualHost {
    Hostname = www.domain.com
    ...
    UseToolkit = rewrite
}
```
****

###Sintassi

La sintassi completa dell'URL toolkit di Hiawatha è:


```
UrlToolkit {
    Call <toolkit_id>
    Match <url> Ban <seconds>
    Match <url> Call <toolkit_id>
    Match <url> DenyAccess
    Match <url> Exit
    Match <url> Expire <time> seconds|minutes|hours|days|weeks|months [Exit|Return]
    Match <url> Goto <toolkit_id>
    Match <url> Redirect <url>
    Match <url> Return
    Match <url> Rewrite <replace> [<max_loop>] [Continue|Return]
    Match <url> Skip <lines>
    Match <url> UseFastCGI <fastcgi_server_id>
    RequestURI exists|isfile|isdir Return|Exit
    Skip <lines>
    ToolkitID = <toolkit_id>
    UseSSL Call <toolkit_id>
    UseSSL Exit
    UseSSL Goto <toolkit_id>
    UseSSL Return
    UseSSL Skip <lines>
}
```


**Spiegazione dei comandi:**

-        Ban: Blocca il client per <seconds> secondi.
-        Call: Esegue un'altra regola e poi continua con la regola corrente..
-        DenyAccess: Impedisce l'accesso al file richiesto (resulta un errore 403) e termina la scansione del toolkit.
-        Exit: Termina l'URL rewriting.
-        Expire: Aggiunge un Expires HTTP header con il timestamp corrente + <time>. Il comportamento di default è di continuare dopo un'azione Expire.
-        Goto: Esegue un'altra regola e termina il processamento del toolkit.
-        Match: Esegue l'azione seguente se <url> matches l'URL richiesto..
-        Redirect: Ridirige il browser all' <url> via un 301 e termina il processamento del toolkit.
-        RequestURI: Verifica se l'URL richiesto è un file o una cartella.
-        Rewrite: Riscrive l'URL corrente per non più di <max_loop> volte (default=1) e termina il processamento del toolkit.
-        Return: Ritorna dalla regola corrente.
-        Skip: Salta le seguenti <number> righe (ToolkitID esclusa).
-        ToolkitID: Assegna un nome all'insieme di regole correnti.
-        UseFastCGI: Usa il server FastCGI con id <fcgi_id> e termina il processamento del toolkit.
-        UseSSL: Esegue l'azione quando un client è connesso via HTTPS.

****

### Wigwam

Hiawatha è fornito di uno strumento detto 'wigwam'. Con questo strumento, è possibile testate le nostre regole di URL toolkit prima di attivarle.

```
wigwam -t <toolkit_id>
```


Pagina originale: [https://www.hiawatha-webserver.org/howto/url_toolkit](https://www.hiawatha-webserver.org/howto/url_toolkit)