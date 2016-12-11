# IP versione 6

Fin dalla versione 6.0, Hiawatha fornisce il supporto per IPv6. Per usare IPv6, dobbiamo definire un collegamento (binding) IPv6:

```
Binding {
    Port = 80
    Interface = ::1
}
```

Questo esempio ci collegherà all'interfaccia IPv6 del localhost.

Se mentre stiamo usando IPv6 riceviamo un errore tentando di connettersi in IPv4 ad un'interfaccia, allora si dovrà usare le configurazioni per i collegamenti IPv4. Così, se questa configurazione:

```
# Binds all IPv4 interfaces:
Binding {
    Port = 80
}

# Bind IPv6 interface:
Binding {
    Port = 80
    Interface = <IPv6 address>
}
```

ci dovesse dare un errore, possiamo provare la seguente configurazione:

```
# Bind IPv4 interfaces:
Binding {
    Port = 80
    Interface = 127.0.0.1
}

Binding {
    Port = 80
    Interface = <IPv4 address>
}

# Bind IPv6 interface:
Binding {
    Port = 80
    Interface = <IPv6 address>
}
```
* * *

### IPv6 FastCGI servers

Per usare un server IPv6 FastCGI, usiamo la seguente notazione per l'opzione **ConnectTo**:

```
FastCGIserver {
    ...
    ConnectTo = [<IPv6 address>]:<port>
}
```

oppure

```
FastCGIserver {
    ...
    ConnectTo = <IPv6 address>.<port>
}
```

Pagina originale: [https://www.hiawatha-webserver.org/howto/ipv6](https://www.hiawatha-webserver.org/howto/ipv6)

