# CGI-WRAPPER

#### NOME

cgi-wrapper - esegue programmi CGI in un ambiente sicuro

- - - -

#### DESCRIZIONE

Il CGI-wrapper può essere usato per eseguire certi programmi CGI con id utente (userid) diverso dallo userid associato
al webserver. Per funzionare correttamente, il binario del CGI-wrapper ha bisogno del su-bit.
Per prevenire abusi, è necessario approntare delle verifiche di sicurezza.
Il CGI-wrapper può essere eseguito dal webserver Hiawatha. Per verificare questo, utilizza il PID-file di Hiawatha PID-file.

- - - -

#### CONFIGURAZIONE

Il CGI-wrapper può essere configurato attraverso il file di configurazione `/etc/hiawatha/cgi-wrapper.conf`.<br/>
Le seguenti opzioni di configurazione sono disponibili:

**CGIhandler = &lt;CGI handler&gt; [, &lt;CGI handler&gt;, ...\]**

Normalmente solo i files che si trovano dentro la WebsiteRoot potranno saranno eseguiti.
I CGI-handlers di solito non si trovano in quella cartella.
Utilizziamo questa opzione per specificare i files binari che sono fuori della WebsiteRoot
ma che il CGI-wrapper è comunque autorizzato ad eseguire.<br/>
Esempio: CGIhandler = /usr/bin/php4-cgi

**Wrap = &lt;wrap_id&gt;;&lt;path&gt;|~&lt;username&gt;;&lt;userid&gt;[:&lt;groupid&gt;[, &lt;groupid&gt;, ...]]**

Attraverso una Wrap-entry, possiamo controllare il CGI-wrapper. Il &lt;wrap_id&gt; è usato per 'collegarlo'
ad un virtual host. Cfr. CGIwrapId in hiawatha(1) per ulteriori informazioni.

La seconda opzione specifica la 'rootdirectory' del programma CGI: il programma deve infatti essere collocato
in questa cartella o in una sua sottocartella. Specificheremo un percorso completo oppure useremo la
homedirectory di un utente + "/public_html/" indicando la sua username preceduta dal carattere '~'.
Nel caso usassimo un path completo, è consigliabile usare la WebsiteRoot del virtual host collegato.
Quando dovessimo specificare un percorso completo potremmo sostituire lo slash con un pipe-sign.
La parte prima del pipe-sign sarà usata per il chroot.
Prestiamo massima attenzione nell'usare un CGI chrooted in combinazione con UserWebsite and Alias
(cfr. hiawatha(1) per ulteriori informazioni riguardo queste opzioni).

Le ultime due opzioni sono la userid e il  groupid del processo CGI.
Se il groupid è omesso, sarà automaticamente cercato in /etc/passwd e /etc/group.
La userid e il  groupid 'root' qui non sono permessi.

Esempio:<br/>
&emsp;&emsp;Wrap = test;/var/www/testsite;testuser<br/>
&emsp;&emsp;Wrap = jail;/usr/jail|sites/public;1001:101

Il CGI-wrapper necessita del pidfile di Hiawatha per funzionare.

Usare "CGIwrapId = un_certo_id" e "Wrap = un_certo_id;~hugo;hugo" è lo stesso di usare "CGIwrapId = ~hugo".

**La maggior parte dei parametri del cgi-wrapper.conf sarebbero già presenti anche in hiawatha.conf.
La ragione per cui devono essere specificati di nuovo e il perché non vengono semplicemente passati da Hiawatha,
è perché se per caso Hiawatha avesse una vulnerabilità, a causa di un bug, ovviamente in una libreria esterna ;),
il CGI-wrapper non possa essere usato per eseguire qualunque programma ci sia sul disco.
E' quindi una scelta fatta per motivi di sicurezza.**

- - - - -

#### VEDI ANCHE

CGI-wrapper è parte del webserver Hiawatha. vedi [hiawatha(1)](https://www.hiawatha-webserver.org/manpages/hiawatha) per ulteriori informazioni riguardo Hiawatha.

- - - - -

#### AUTORE

Hugo Leisink <hugo@hiawatha-webserver.org> - [http://www.hiawatha-webserver.org/](http://www.hiawatha-webserver.org/)


Pagina originale: [https://www.hiawatha-webserver.org/manpages/cgi-wrapper](https://www.hiawatha-webserver.org/manpages/cgi-wrapper)