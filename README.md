# DownRightNow
Simple github-page to redirect to when doing maintenance.

### Nginx Configuration

```nginx
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    
    server_name example.com;

    location '/.well-known/acme-challenge' {
        default_type "text/plain";
        root /tmp/letsencrypt-auto;
    }
    location / {
        add_header Strict-Transport-Security max-age=15768000;
        return 301 https://$host$request_uri;
    }

    # Redirect non-https traffic to https
    # if ($scheme != "https") {
    #     return 301 https://$host$request_uri;
    # } # managed by Certbot
   
}

server {
    listen 443 ssl;
    server_name example.com;
    ssl on;
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem; # managed by Certbot
    client_max_body_size 100M;
    location / {
        proxy_pass https://codeforafrica.github.io/DownRightNow/;
        resolver 8.8.8.8;
        proxy_set_header Accept-Encoding "";
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

}
```
