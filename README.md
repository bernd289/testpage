# testpage

this is an alternative to http://test.schrimpe.de/

You can test it here:

https://test.dssr.ch

https://test.matthiashille.com 
(IPv4 only: https://test4.matthiashille.com IPv6 only: https://test6.matthiashille.com)

https://test.mazdermind.de 
(IPv4 only: https://test4.mazdermind.de IPv6 only: https://test6.mazdermind.de)


## Setup

### Apache

1. Activate SSI in Apache: https://httpd.apache.org/docs/current/mod/mod_include.html


2. Activate Hostname Lookup in Apache.

Therefore change the following line from "Off" to "On" in the main apache2 conf, e.g. on Ubuntu

Original:
`HostnameLookups Off`

Change to:
`HostnameLookups On`

### Nginx

1. Check that your Nginx has the [http_sub_module](https://nginx.org/en/docs/http/ngx_http_sub_module.html) enabled:

```shell
nginx -V | grep -q http_sub_module && echo "http_sub_module enabled" || echo "http_sub_module disabled"
```

2. Configure an nginx site with the replacements:

```
geo $ipversion {
        ::0/0 ipv6;
        0.0.0.0/0 ipv4;
}

server {
    server_name .test.example.com;

    listen 80;
    listen [::]:80;
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    root /var/www/testpage;
    index index.shtml;

    location / {
        sub_filter '<!--#echo var="DATE_LOCAL" -->' '$date_local';
        sub_filter '<!--#echo var="DATE_GMT" -->' '$date_gmt';
        sub_filter '<!--#echo var="HTTP_ACCEPT_LANGUAGE" -->' '$http_accept_language';
        sub_filter '<!--#echo var="HTTP_ACCEPT" -->' '$http_accept';
        sub_filter '<!--#echo var="HTTP_CONNECTION" -->' '$http_connection';
        sub_filter '<!--#echo var="HTTP_IPVERSION" -->' '$ipversion';
        sub_filter '<!--#echo var="HTTP_USER_AGENT" -->' '$http_user_agent';
        sub_filter '<!--#echo var="HTTPS" -->' '$https';
        sub_filter '<!--#echo var="REMOTE_ADDR" -->' '$remote_addr';
        sub_filter '<!--#echo var="REMOTE_HOST" -->' '';
        sub_filter '<!--#echo var="REMOTE_PORT" -->' '$remote_port';
        sub_filter '<!--#echo var="REQUEST_METHOD" -->' '$request_method';
        sub_filter '<!--#echo var="SERVER_NAME" -->' '$host';
        sub_filter '<!--#echo var="SERVER_ADDR" -->' '$server_addr';
        sub_filter '<!--#echo var="SERVER_PORT" -->' '$server_port';
        sub_filter '<!--#echo var="SERVER_PROTOCOL" -->' '$server_protocol';
        sub_filter '<!--#echo var="SSL_PROTOCOL" -->' '$ssl_protocol';
        sub_filter '<!--#echo var="SSL_CIPHER" -->' '$ssl_cipher';
        sub_filter '<!--#echo var="SSL_TLS_SNI" -->' '$ssl_server_name';
        sub_filter_once off;
    }
}
```
