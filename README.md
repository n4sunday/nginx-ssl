<img src="https://www.nginx.com/wp-content/uploads/2020/05/NGINX-product-icon.svg" width="100" height="100"/>

### NGINX
```bash
├── nginx
│   ├── docker-compose.yml
│   ├── default.conf
│   └── ssl
│       ├─ certificate.crt
│       └─ certificate.key

```


*docker-compose.yml*
```yml

version: '3.1'
services:
  nginx:
    image: nginx
    container_name: nginx
    restart: always
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./default.conf:/etc/nginx/conf.d/default.conf
      - ./ssl/bscp.crt:/etc/nginx/certs/certificate.crt
      - ./ssl/bscp.key:/etc/nginx/certs/certificate.key
```

*default.conf*
```bash
server {
   listen 80;
   server_name www.domain.com;

   return 301 https://www.domain$request_uri;
}

server {
    listen 443 ssl;
    server_name www.domain.com;

    ssl_certificate       /etc/nginx/certs/certificate.crt;
    ssl_certificate_key   /etc/nginx/certs/certificate.key;

    location / {
        proxy_pass http://domain:port;
    }
}
```

#### Cloudflare
##### Create Certificate 
> SSL/TLS > Client Certificates > Create Certificate

##### Create Sub Domain
| Type      | Name           | IPv4 address | TTL  |
| --------- | -------------- | ------------ | ---- |
| **CNAME** | **sub.domain** | @            | Auto |

