## install nginx

```bash
sudo apt-get install -y nginx
run ip, not working then htto open security
cd /etc/nginx/sites-available

sudo vim default 

location /api {
    rewrite ^\/api\/(.*)$ /api/$1 break;
    proxy_pass http://localhost:3000;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}

sudo systemctl restart nginx

```

## Add domain to nginx configuration

```bash
server_name shopdev.example.com www.shopdev.example.com

location / {
    proxy_pass http://localhost:3000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
}
```

## add SSL to domain
```bash
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python3-certbot-nginx
sudo certbot --nginx -d domain.example.com
sudo certbot renew --dry-run #lệnh tự động gia hạn domain
sudo systemctl status certbot.timer
```
