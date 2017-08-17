## UNMS installation parameters:

    $ curl -fsSL https://raw.githubusercontent.com/Ubiquiti-App/UNMS/master/install.sh > /tmp/unms_install.sh
    $ sudo bash /tmp/unms_install.sh --behind-reverse-proxy --public-https-port 443 --http-port 8080 --https-port 8443 

## Nginx config

*change `server_name`, `ssl_certificate`, `ssl_certificate_key` and `proxy_pass` to match your environment*

    map $http_upgrade $connection_upgrade {
      default upgrade;
      ''      close;
    }
    
    server {
      listen 80;
      server_name unms.example.com;

      client_max_body_size 4G;
    
      location / {
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_pass http://127.0.0.1:8080/;
      }
    }
    
    server {
      listen 443 ssl http2;
      server_name unms.example.com;
    
      ssl_certificate     /etc/letsencrypt/live/unms.example.com/fullchain.pem;
      ssl_certificate_key /etc/letsencrypt/live/unms.example.com/privkey.pem;
    
      ssl on;
    
      set $upstream 127.0.0.1:8443;
    
      location / {
        proxy_pass     https://$upstream;
        proxy_redirect https://$upstream https://$server_name;
    
        proxy_cache off;
        proxy_store off;
        proxy_buffering off;
        proxy_http_version 1.1;
        proxy_read_timeout 36000s;
    
        proxy_set_header Host $http_host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Referer "";
    
        client_max_body_size 0;
      }
    }

## Apache config

*change `ServerName`, `SSLCertificateFile` and `SSLCertificateKeyFile` to match your environment*

    <VirtualHost *:80>
      ProxyPreserveHost On
      ProxyPass / http://127.0.0.1:8080/
      ProxyPassReverse / http://127.0.0.1:8080/
    </VirtualHost>
    
    <VirtualHost *:443>
      ProxyRequests Off
      ProxyPreserveHost On
    
      ServerName unms.example.com
    
      SSLEngine on
      SSLCertificateFile /etc/letsencrypt/live/unms.example.com/fullchain.pem
      SSLCertificateKeyFile /etc/letsencrypt/live/unms.example.com/privkey.pem
    
      SSLProxyEngine On
      SSLProxyVerify none
      SSLProxyCheckPeerCN off
      SSLProxyCheckPeerExpire off
      SSLProxyCheckPeerName off
    
      RewriteEngine on
      RewriteCond %{HTTP:Connection} Upgrade [NC]
      RewriteCond %{HTTP:Upgrade} websocket [NC]
      RewriteRule /(.*) wss://127.0.0.1:8443/$1 [P,L]
    
      ProxyPass / https://127.0.0.1:8443/
      ProxyPassReverse / https://127.0.0.1:8443/
    </VirtualHost>