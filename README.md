
<p align=center>
 <img src="https://www.nginx.com/wp-content/uploads/2020/05/NGINX-product-icon.svg" width="120"/>
 </p>

<p align=center>
 <img src="https://raw.githubusercontent.com/docker-library/docs/01c12653951b2fe592c1f93a13b4e289ada0e3a1/nginx/logo.png" width="120"/>
</p>


```bash
├── nginx
│   ├── docker-compose.yml
│   ├── default.conf
│   └── ssl
│       ├─ certificate.crt
│       └─ certificate.key

```


#### `docker-compose.yml`
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
      - ./ssl/certificate.crt:/etc/nginx/certs/certificate.crt
      - ./ssl/certificate.key:/etc/nginx/certs/certificate.key
```
**Port**
`80` for Hypertext Transfer Protocol (HTTP)
`443` for (HTTP over an SSL/TLS) HTTPS

**SSL volumns certificate**
  - ./ssl/certificate.crt:/etc/nginx/certs/certificate.crt
  - ./ssl/certificate.key:/etc/nginx/certs/certificate.key

#### `default.conf`

```bash
server {
   listen 80;
   server_name domain.com;

   return 301 https://domain.com$request_uri;
}

server {
    listen 443 ssl;
    server_name domain.com;

    ssl_certificate       /etc/nginx/certs/certificate.crt;
    ssl_certificate_key   /etc/nginx/certs/certificate.key;

    location / {
        proxy_pass http://private_ip:port;
    }
}
```
#### `certificate.crt`

```bash
-----BEGIN CERTIFICATE-----
XXXXXXXXXXXX
-----END CERTIFICATE-----
```
#### `certificate.key`

```bash
-----BEGIN PRIVATE KEY-----
XXXXXXXXXXXX
-----END PRIVATE KEY-----
```

<p align=center>

 <img src="https://www.cloudflare.com/img/logo-cloudflare-dark.svg" width="250" />

 </p>
#### Cloudflare
##### Create Certificate 
 `SSL/TLS` > `Client Certificates` > `Create Certificate`

Result file from nginx
- `certificate.crt`
- `certificate.key`
##### Create Sub Domain
| Type      | Name         | IPv4 address | TTL  |
| --------- | ------------ | ------------ | ---- |
| **CNAME** | `sub.domain` | @            | Auto |

