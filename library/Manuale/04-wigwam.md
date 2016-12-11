# WIGWAM

#### NOME

wigwam - verifica la configurazione del webserver Hiawatha per errori non-critici.

- - - -

#### SINOSSI

wigwam [opzioni]

```
Opzioni:
          -b <username [<password]: crea una nuova voce nel file delle password per l'autenticazione basic HTTP.
          -c percorso: percorso per raggiungere il file di configurazione.
          -d <username <realm [<password]: crea una nuova voce nel file delle password per l'autenticazione digest HTTP.
          -h: visualizza la schermata di help ed esce.
          -q: non visualizza i risultati del test.
          -s: stampa gli hash dei file nella cartella corrente.
          -t <toolkit_id: testa le regole dell'URL toolkit specificato.
          -v: visualizza la versione ed esce.
```

- - - - -

#### DESCRIZIONE

Wigwam è uno strumento di validazione per il webserver Hiawatha. Lo usiamo per controllare il file di configurazione
principale alla ricerca di errori non-fatali. Questo strumento carica la configurazione in una maniera diversa rispetto
a come lo farebbe Hiawatha in modo da rendere più semplice l'individuazione di certi errori.<br/>
Questi controlli sono stati messi in un programma separato al fine di contenere le dimensioni di Hiawatha.

- - - - -

#### URL TOOLKIT TESTING

Con l'opzione `-t`, possiamo usare Wigwam per testare il nostro set di regole URL toolkit. Per testare le regole
dell'URL toolkit, noi dovremmo inserire solo la parte dopo l'hostname nella URL.<br/>
Ad esempio, se noi volessimo testare l'URL "http://www.mydomain.com/index.php?key=value",
dovremmo inserire "index.php?key=value".<br/>
Un risultato visualizzato come "old: <URL\>" significa che niente è cambiato.
Quando una toolkit rule è stata applicata il risultato è visualizzato come "new: <URL\>".

Quando stiamo testando la Header UrlToolkit rule, noi potremmo settare le variabili
d'ambiente HTTP_HOST, HTTP_REFERER and HTTP_USER_AGENT per simulare gli headers Host, Referer e User-Agent HTTP.

Infine, per poter testare un URL toolkit, Hiawatha **NON** deve essere stato compilato con `-DENABLE_TOOLKIT=off`.<br/>
Usiamo il comando `'hiawatha -v'`  da linea di comando per le informazioni di compilazioni utilizzate.

- - - -

#### VEDI ANCHE

Wigwam è parte del webserver Hiawatha. vedi [hiawatha(1)](https://www.hiawatha-webserver.org/manpages/hiawatha) per ulteriori informazioni riguardo Hiawatha.

- - - - -

#### AUTORE

Hugo Leisink <hugo@hiawatha-webserver.org> - [http://www.hiawatha-webserver.org/](http://www.hiawatha-webserver.org/)


Pagina originale: [https://www.hiawatha-webserver.org/manpages/wigwam](https://www.hiawatha-webserver.org/manpages/wigwam)