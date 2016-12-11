# Compilazione e installazione

### Introduzione

La prima sezione qui sotto descrive le istruzioni su come compilare e installare Hiawatha partendo dal codice sorgente
per la maggior parte dei sisyemi operativi Linux, BSD e UNIX. Comunque è assolutamente raccomandabile installare i binari
pre-compilati per il tuo particolare sistema operativo, usando gli strumenti messi a disposizione dal sistema operativo stesso.
La seconda sezione include invece le istruzioni su come compilare e installare Hiawatha partendo dai sorgenti per quello
che riguarda le distribuzioni Linux Debian o Debian-based (tipo Ubuntu).

La localizzazione dei vari files di Hiawatha dipende dal metodo di installazione adoperato e dal sistema operativo.
Quando si installa Hiawatha usando un pacchetto Linux, i binari sono generalmente posizionasti in `/usr/sbin` e i files
di configurazione sono in `/etc/hiawatha`.
Quando invece si installa su un sistema BSD via port o compilando i sorgenti, i binari sono di solito in
`/usr/local/sbin` e i files di configurazioine in `/usr/local/etc/hiawatha`.

Per verificare quali siano le effettive posizioni dei binari di Hiawatha, dei files .conf e delle man pages
(pagine del manuale), dopo l'installazione, sempre come utente root eseguiamo i seguenti comandi:

```
updatedb
whereis hiawatha cgi-wrapper newroot php-fcgi wigwam
```

****

### Compilare e installare partendo dai sorgenti

Per poter compilare e installare Hiawatha partendo dai sorgenti, è necessario che sul nostro sistema sia installato un set completo di strumenti per compilare il C.
A questo proposito, dobbiamo consultare la documentazione del nostro sistema operativo per vedere come installare questo software.

Inoltre, sul nostro sistema, per poter sfruttare appieno le funzionalità di Hiawatha, dobbiamo aver installato anche le librerie `libxml2` e `libxslt`.

Hiawatha necessita di [CMake](http://www.cmake.org/) per la compilazione. Scarichiamo l'[ultima versione](http://www.cmake.org/cmake/resources/software.html)
e installiamola prima di compilare Hiawatha.

A questo punto, scarichiamoci i sorgenti dalla [pagina di download](https://www.hiawatha-webserver.org/download).
Unzippiamo l'archivio dei sorgenti e entriamo nella cartella hiawatha-<versione> dando i seguenti comandi:


```
tar -xzf hiawatha-<versione>.tar.gz
cd hiawatha-<versione>
```

Per preparare il codice sorgente Hiawatha per la compilazione, eseguiamo i seguenti comandi:

```
cd hiawatha-<versione>
mkdir build
cd build
cmake ..
```

Di seguito le opzioni disponibili per il CMake. In **maiuscolo** quelle di default.

```
-DENABLE_CACHE=ON|off 	Enable cache support in Hiawatha
-DENABLE_DEBUG=on|OFF 	Enable debug information (for development only)
-DENABLE_IPV6=ON|off 	Enable IPv6 support in Hiawatha
-DENABLE_MONITOR=on|OFF 	Enable support for the Hiawatha Monitor
-DENABLE_RPROXY=ON|off 	Enable reverse proxy support in Hiawatha
-DENABLE_SSL=ON|off 	Enable SSL (PolarSSL) support in Hiawatha
-DENABLE_TOMAHAWK=on|OFF 	Enable Tomahawk in Hiawatha
-DENABLE_TOOLKIT=ON|off 	Enable the URL toolkit in Hiawatha
-DENABLE_XSLT=ON|off 	Enable XSLT support in Hiawatha
```
 

I seguenti settaggi per i percorsi (path) sono disponibili per il CMake.

```
-DCMAKE_INSTALL_PREFIX=<path> 	The prefix for all other CMAKE_INSTALL directories
-DCMAKE_INSTALL_BINDIR=<path> 	Location of the ssi-cgi binary
-DCMAKE_INSTALL_SBINDIR=<path> 	Location of the other Hiawatha binaries
-DCMAKE_INSTALL_SYSCONFDIR=<path> 	The configuration files will be installed in /hiawatha
-DCMAKE_INSTALL_LIBDIR=<path> 	The PolarSSL shared library will be installed in /hiawatha
-DCMAKE_INSTALL_MANDIR=<path> 	Manual pages will be installed in /man1
-DCONFIG_DIR=<path> 	Location of the Hiawatha configuration files
-DLOG_DIR=<path> 	Log directory used in the default hiawatha.conf
-DPID_DIR=<path> 	Location of the Hiawatha and php-fcgi PID files
-DWEBROOT_DIR=<path> 	Webroot directory used in the default hiawatha.conf
-DWORK_DIR=<path> 	Path of directory where Hiawatha can write temporary files
```


Per compilare Hiawatha, eseguiamo il seguente comando:

```
make
```

Poi, come utente root, installiamo Hiawatha con il seguente comando:

```
make install/strip
```

****


### Verifichiamo che tutto funzioni

Se abbiamo effettuato un upgrade di una precedente versione di Hiawatha, eseguiamo il seguente comando per assicurarci che il file `hiawatha.conf` sia
compatibile con il binario appena installato:

```
hiawatha -k
```

Se tutto va bene, se cioè il file hiawatha.conf è compatibile con la versione di Hiawatha appena installata, otterremo questo output:

```
Using <percorso del file hiawatha.conf>
Reading hiawatha.conf
Configuration OK
```

Se la verifica fallisce, allora andiamo a rivedere la documentazione di Hiawatha e [usiamo la funzione di ricerca sul sito ufficiale di Hiawatha](https://www.hiawatha-webserver.org/search).
Se nonostante questo non riuscissimo ancora a trovare una soluzione, allora potremmo postare la nostra domanda sul [forum](https://www.hiawatha-webserver.org/forum).

****

### Compiliamo un pacchetto Debian

Per creare un pacchetto Debian, eseguiamo il seguente script partendo dalla directory che contiene i sorgenti seguendo le istruzioni sullo schermo:

```
./extra/make_debian_package
```

Per installare il pacchetto, come utente root eseguiamo il seguente comando:

```
dpkg -i hiawatha_<versione>_<architettura>.deb
```

Se stiamo effettuando un aggiornamento da una versione precedente di Hiawatha, eseguiamo il seguente comando per assicurarci che il file `hiawatha.conf` sia compatibile con la versione di Hiawatha appena installata:

```
hiawatha -k
```

Se tutto va bene, se cioè il file `hiawatha.conf` è compatibile con la versione di Hiawatha appena installata, otterremo questo output:

```
Using <percorso del file hiawatha.conf>
Reading hiawatha.conf
Configuration OK
```

Se la verifica fallisce, allora andiamo a rivedere la documentazione di Hiawatha e [usiamo la funzione di ricerca sul sito ufficiale di Hiawatha](https://www.hiawatha-webserver.org/search).
Se nonostante questo non riuscissimo ancora a trovare una soluzione, allora potremmo postare la nostra domanda sul [forum](https://www.hiawatha-webserver.org/forum).

Per rimuovere un pacchetto .deb installato col comando `dpkg`, come utente root eseguiamo il seguente comando:

```
dpkg --purge hiawatha
```

Nota che questo comando rimuoverà tutti i binari Hiawatha compresi tutti i files di configurazione e le man pages (pagine del manuale).


Pagina originale: [https://www.hiawatha-webserver.org/howto/compilation_and_installation](https://www.hiawatha-webserver.org/howto/compilation_and_installation)