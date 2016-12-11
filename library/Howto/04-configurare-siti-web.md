# Configurare siti web

Per far si che Hiawatha sia in grado di servire il nostro sito, useremo WebsiteRoot per specificarne la posizione e Hostname per indicarne il nome host. Questo sito web sarà conosciuto come il 'default website' ovvero il sito di default, perché  sarà quello che Hiawatha ci mostrerà quando un client dovesse richiedere un sito non configurato in Hiawatha (unknown hostname).

```
Hostname = <IP address of server>
WebsiteRoot = /var/www/dummy
```

Creiamo il file /var/www/dummy/index.html mettendoci dentro un po' di HTML. Avviamo il webserver Hiawatha  e puntiamo il nostro browser al server dove Hiawatha è in esecuzione. Se tutto va come dovrebbe, dovremmo veder apparire la nostra pagina web.

Sarebbe cosa furba usare l'indirizzo IP del server come hostname del server di default a cui facciamo corrispondere una pagina vuota. Nella maggior parte dei casi gli  hackers usano l'indirizzo IP come hostname mentre effettuano le scansioni dei webserver alla ricerca di siti web vulnerabili. Facendo in questa maniera, gli hacker finiranno su una pagina vuota invece di finire sulla nostra applicazione web che potrebbe avere invece qualche vulnerabilità.

****

### Siti virtuali (Virtual hosts)

Hiawatha può naturalmente servire più di un sito web. Ma siccome ci può essere un solo sito di default, gli altri devono essere 'virtuali'.  Ne possiamo attivare uno aggiungendo una sezione virtual host al nostro file di configurazione.

```
VirtualHost {
    Hostname = www.mywebsite.com
    WebsiteRoot = /var/www/website1/wwwroot
}
```

Quasi sicuramente vorremmo che i file di log di questo sito web finiscano in un file separato. Questo può essere fatto aggiungendo l'opzione logfile alla sezione virtual host che abbiamo appena creato.

```
VirtualHost {
    ...
    AccessLogfile = /var/www/website1/log/access.log
    ErrorLogfile = /var/www/website1/log/error.log
}
```

Se index.html non è il file indice di default del sito, useremo l'opzione StartFile per definirne uno alternativo.

```
VirtualHost {
    ...
    StartFile = start.html
}
```

****

### Processo di selezione

Quale configurazione di host userà Hiawatha per una certa richiestat? In questo processo di selezione, Hiawatha comincerà cercando di individuare l'hostname nella richiesta. Questo sarà ovviamente la parte hostname della URL richiesta. Per esempio, per la richiesta di questa pagina (http://www.hiawatha-webserver.org/howto/websites) la parte 'hostname' sarà www.hiawatha.it.

A questo punto, Hiawatha tenterà di trovare un hostname che coincida in tutte le configurazioni di host (sia default che virtuali). Alla fine, se non trovasse un hostname che coincida,  Hiawatha userà l'hostaname di default.


Pagina originale: [https://www.hiawatha-webserver.org/howto/websites](https://www.hiawatha-webserver.org/howto/websites)