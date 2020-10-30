# Docker Monitoring Bundle

### Requirement

    🐳  docker
    🐙  docker-compose
    🐱  git
    🐶  nginx
    🐭  certbot

---

### 🐳 Docker

[Docker Installation](https://docs.docker.com/engine/install/)

> $ sudo apt-get update
> $ sudo apt-get install docker-ce docker-ce-cli containerd.io

---

### 🐙 Docker compose

[Docker-compose Installation](https://docs.docker.com/compose/install/)

> $ sudo curl -L"https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)"-o /usr/local/bin/docker-compose
> $ sudo chmod +x /usr/local/bin/docker-compose

---

### 🐱 Git

[Git Installation](https://www.atlassian.com/git/tutorials/install-git)

> \$ sudo apt-get update; sudo apt-get install git

---

### 🐶 Nginx

[Nginx Installation](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04-quickstart)

> $ sudo apt update
> $ sudo apt install nginx
>
> \$ sudo systemctl status nginx

---

### 🐭 Certbot

[Certbot Installation](https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx)

> $ sudo snap install core; sudo snap refresh core
> $ sudo snap install --classic certbot
> $ sudo ln -s /snap/bin/certbot /usr/bin/certbot
> $ sudo certbot certonly --nginx

🚀 Then... location of certificates in /etc/letsencrypt/live/push.nextschool.io/

---

### ⚠️ !! IF YOU WANT TO UNINSTALL NGINX

> \$ sudo systemctl stop nginx

> \$ sudo systemctl status nginx

> \$ sudo apt-get purge nginx nginx-common

---

### 👽 Docker-monitoring-bundle

    git clone: https://github.com/Winz-Mawin7/docker-monitoring-bundle

1.  **Kong** 🦍

    > \$ sudo sh -c 'cd Kong && docker-compose up'

Wait until Kong start...

- Config by Konga GUI: PORT 1337

  - Add Services
  - Add Routes
  - Add Certificate

- Config by REST API
  - Add Certificate by curl to API

```
$ curl -i -m 60 -X POST http://localhost:8001/certificates  \
-F "cert=$(sudo cat /etc/letsencrypt/live/DOMAIN.NAME/cert.pem)" \
-F "key=\$(sudo cat /etc/letsencrypt/live/DOMAIN.NAME/privkey.pem)" \
-F "snis=['DOMAIN.NAME}]"
```

---

2. **Monitoring** 📈

> \$ sudo cat /etc/letsencrypt/live/DOMAIN.NAME/cert.pem >> ~/docker-monitoring-bundle/Monitoring/certs/cert.pem

> \$ sudo cat /etc/letsencrypt/live/DOMAIN.NAME/privkey.pem >> ~/docker-monitoring-bundle/Monitoring/certs/privkey.pem

> \$ sudo sh -c 'cd Monitoring && docker-compose up'

3.  **Services** 🐯

> \$ sudo sh -c 'cd Services && docker-compose up'

---

### Finally

- Grafana
  - Add Data Source
  - Import JSON file from folder Monitoring/Dashboard

---

### PORTS (Open World)

- 3000 : Grafana
- 5432 : Postgresql

- 8000 : Kong HTTP
- 8443 : KONG HTTPS

- 9090 : Prometheus
- 9100 : Node_Exporter

- 8088: Node-FCM
