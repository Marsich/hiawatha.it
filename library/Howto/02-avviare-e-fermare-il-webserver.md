# Avviare e fermare il webserver


### Avviare e Fermare (Start/Stop) Hiawatha installato attraverso la compilazione dei sorgenti

#### Start

Per prima cosa bisogna individuare la locazione del binario di Hiawatha eseguendo il seguente comando:

```
whereis hiawatha
```

Per avviare il webserver Hiawatha, bisogna come utente root eseguire il seguente comando:

```
/percorso/al/file/binario/hiawatha
```

****

#### Stop

Per fermare il webserver Hiawatha, per prima cosa dobbiamo trovare il PID del processo usando il seguente comando:

```
cat /var/run/hiawatha.pid
```

Quindi come utente root eseguiamo il seguente comando:

```
kill -15 <hiawatha PID number>
```

****

### Avviare e Fermare (Start/Stop) Hiawatha installato attraverso il Debian Package

#### Start

Per avviare Hiawatha webserver, come root eseguiamo il seguente comando:

```
/etc/init.d/hiawatha start
```

#### Stop

Per terminare Hiawatha webserver, come root eseguiamo invece il seguente comando:

```
/etc/init.d/hiawatha stop
```

Pagina originale: [https://www.hiawatha-webserver.org/howto/starting_and_stopping](https://www.hiawatha-webserver.org/howto/starting_and_stopping)