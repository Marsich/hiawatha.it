---
"title": "Configurazione PHP",
"author": "Paolo Marsi",
"description": "Hiawatha webserver - Configurazione PHP",
"tags": ["hiawatha", "manuale", "italiano", "configurazione PHP"]
---

## Configurazione PHP

Quando usiamo Hiawatha col PHP, assicuriamoci che i seguenti settaggio sia correttamente impostati nel nostro php.ini:

    cgi.fix_pathinfo = 0
    cgi.rfc2616_headers = 0

I settaggi seguenti non sono obbligatori ma solo raccomandati:

    # Enable GZip content encoding
    zlib.output_compression = On
    zlib.output_compression_level = 6

    # Security settings
    expose_php = Off
    display_errors = Off
    register_globals = Off
    magic_quotes_gpc = Off
    allow_url_include = Off

Pagina originale: [https://www.hiawatha-webserver.org/howto/php_configuration](https://www.hiawatha-webserver.org/howto/php_configuration)