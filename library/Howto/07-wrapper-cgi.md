# Wrapper CGI

Il CGI-wrapper può essere usato per eseguire programmi CGI con un useri/groupid diversi da quelli associate al webserver.
Può essere anche usato per eseguire programmi CGI in un ambiente chrooted.
La configurazione del CGI-wrapper è inserita nel file cgi-wrapper.conf che è posizionato nella cartella dei file di configurazione di Hiawatha.
Ogni wrapping CGI deve essere configurato attraverso l'opzione Wrap. Il formato dell'opzione '**Wrap**' è:


```
Wrap = <id of this wrap>:<CGI rootdirectory>:<userid to change to>
```


Il CGI-wrapper eseguirà solo i programmi che sono posizionati all'interno della cartella radice CGI. Se la specificazione della cartella radice CGI contiene un segno 'pipe' ( | barra verticale ) la parte prima del pipe sarà usata come cartella chroot. L'opzione CGIhandler può essere usata per specificare programmi esterni alla cartella root CGI che il CGI-wrapper è autorizzato ad eseguire, come per esempio un programma php-cgi. Se usiamo un wrap chrooted (col segno pipe), assicuriamoci che il corretto CGIhandlers sia disponibile anche all'interno di questa cartella chroot.


```
# cgi-wrapper.conf
CGIhandler = /usr/bin/php5-cgi
CGIhandler = /usr/bin/perl

Wrap = wrap_id:/var/www:hugo
Wrap = chroot:/usr/chroot|www:hugo
```

Usiamo l'opzione **WrapCGI**  per connettere un 'wrap' ad un virtual host.

```
# hiawatha.conf
CGIwrapper = /usr/sbin/cgi-wrapper

VirtualHost {
  ...
  WrapCGI = wrap_id
}

VirtualHost {
  ...
  WrapCGI = chroot
}
```


Pagina originale: [https://www.hiawatha-webserver.org/howto/cgi_wrapper](https://www.hiawatha-webserver.org/howto/cgi_wrapper)