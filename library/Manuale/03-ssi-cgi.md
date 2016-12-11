# SSI-CGI

#### NOME

ssi-cgi - Server Side Includes parser as a CGI program

- - - - -

#### SINOSSI

ssi-cgi<br/>
-h: visualizza l'help ed esce.<br/>
-v: mostra versione e informazioni di compilazione ed esce.<br/>

- - - -

#### DESCRIZIONE

SSI parser che funziona come un'applicazione CGI.

- - - -

#### COMANDI SSI

I comandi SSI hanno il seguente formato: `<!--#command parameter="value"-->`.<br/>
Questa sezione mostrerà tutti i comandi SSI disponibili e i loro parametri in ssi-cgi.


**config**<br/>
&emsp;&emsp;Usiamo il comando config per controllare l'output.<br/>
&emsp;&emsp;Parametri:<br/>
&emsp;&emsp;**errmsg**: Configura il messaggio che ssi-cgi deve visualizzare in caso di errore.<br/>
&emsp;&emsp;Esempio: <!--#config errmsg="SSI error"--\>

**echo**<br/>
  &emsp;&emsp;Stampa il valore di una variabile.<br/>
  &emsp;&emsp;Parametri:<br/>
  &emsp;&emsp;**var**: Stampa il valore di una variabile d'ambiente.<br/>
  &emsp;&emsp;Esempio: <!--#echo var="DOCUMENT_ROOT"--\>

**exec**<br/>
  &emsp;&emsp;Esegue un programma e include il suo output nella posizione del comando exec.<br/>
  &emsp;&emsp;Parametri:<br/>
  &emsp;&emsp;**cgi**: Esegue un programma CGI relativo alla root del sito.<br/>
  &emsp;&emsp;**cmd**: Esegue un qualunque comando come un parametro di /bin/sh.<br/>
  &emsp;&emsp;Esempio: <!--#exec cmd="ls -Flsh"--\>

**fsize**<br/>
  &emsp;&emsp;Stampa la dimensione di un file.<br/>
  &emsp;&emsp;Parametri:<br/>
  &emsp;&emsp;**file**: Il file del quale vogliamo venga stampata la dimensione.<br/>
  &emsp;&emsp;Esempio: <!--#fsize file="info.txt"--\>

**include**<br/>
  &emsp;&emsp;Include un file nella posizione del comando di inclusione.<br/>
  &emsp;&emsp;Parametri:<br/>
  &emsp;&emsp;**file**: Include un file relativo al file corrente.<br/>
  &emsp;&emsp;**virtual**: Include un file relativo alla root del sito.<br/>
  &emsp;&emsp;Esempio: <!--#include file="information.txt"--\>

**printenv**<br/>
  &emsp;&emsp;Stampa tutte le variabili d'ambiente.<br/>
  &emsp;&emsp;Esempio: <!--#printenv--\>

- - - - -

#### VEDI ANCHE

SSI-CGI è parte del webserver Hiawatha. vedi [hiawatha(1)](https://www.hiawatha-webserver.org/manpages/hiawatha) per ulteriori informazioni riguardo Hiawatha.

- - - - -

#### AUTORE

Hugo Leisink <hugo@hiawatha-webserver.org> - [http://www.hiawatha-webserver.org/](http://www.hiawatha-webserver.org/)


Pagina originale: [https://www.hiawatha-webserver.org/manpages/ssi-cgi](https://www.hiawatha-webserver.org/manpages/ssi-cgi)