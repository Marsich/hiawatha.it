# Cartella customizzata e layout XML

Hiawatha dispone del supporto XSLT. Permette di trasformare dei file XML via XSLT e visualizzare il contenuto di una cartella customizzata via XSLT.

### XML files
Richiamando files XML, questi possono essere trasformati via XSLT.<br/>
Se un virtual host ha il supporto XSLT abilitato, Hiawatha cercherà un file .xslt che coincida col filename del file .xml.<br/>
Ad esempio, se venisse richiesto file.xml, Hiawatha cercherà un file con nome file.xslt e lo userà per trasformare il file XML.

Se non venisse trovato un corrispondente file XSLT, Hiawatha cercherà di trovare un file index.xslt nella stessa cartella.<br/>
Se ancora non trovasse niente, Hiawatha tenterà di trovare un index.xslt nella cartella principale del sito.

Se un XSLT non venisse trovato, lo stesso file XML sarà inviato al client.

Per abilitare il supporto XSLT per un virtual host, utilizzeremo il seguente settaggio:

```
VirtualHost {
  ...
  UseXSLT = yes
}
```

****

### Directory index

Quando richiediamo la lista di una cartella e ShowIndex è settato a 'yes', Hiawatha creerà un file XML file del contenuto della cartella e lo usa con un file XSLT per realizzare la visualizzazione. Il file XSLT di default può essere trovato nella cartella delle configurazioni di Hiawatha. Se volessimo usare un differente file XSLT, utilizziamo il percorso a quel particolare file XSLT come parametro dell'opzione ShowIndex.

```
VirtualHost {
  ...
  ShowIndex = /path/to/layout.xslt
}
```

Un esempio di file XML generato da Hiawatha è presentato qui sotto:

```
<index>
  <hostname>www.example.com</hostname>
  <request_uri>/documents/</request_uri>
  <files>
    <file type="dir" timestamp="13 Jun 2009, 12:00:00" url_encoded="..">../</file>
    <file type="file" timestamp="20 Jun 2009, 15:30:00" size="1.4 MB" extension="pdf"
          url_encoded="file.pdf">file.pdf</file>
    <file type="file" timestamp="01 Jul 2009, 20:15:00" size="372.9 kB" extension="txt"
          url_encoded="info.txt">file.txt</file>
  </files>
  <total_size>1.77 MB</total_size>
  <software>Hiawatha v7.3</software>
</index>
```

Pagina originale: [https://www.hiawatha-webserver.org/howto/customized_layout](https://www.hiawatha-webserver.org/howto/customized_layout)