# Docker Monitoring Bundle

### Requirement

    🐳  docker
    🐙  docker-compose
    🐱  git
    🐶  nginx
    🐭  certbot

---

### 🔥 Ubuntu

```
sudo apt update && sudo apt upgrade -y
```

---

### 🐳 Docker

[Docker Installation](https://docs.docker.com/engine/install/)

Ubuntu

```
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io
```

---

### 🐙 Docker compose

[Docker-compose Installation](https://docs.docker.com/compose/install/)

Ubuntu

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

---

### 🐱 Git

> Using git for download my repository otherwise you can use another way...

[Git Installation](https://www.atlassian.com/git/tutorials/install-git)

Ubuntu

```
sudo apt-get install git
```

---

### 🐶 Nginx

> Using Nginx for pretend as web server and give Certbot to check.

[Nginx Installation](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04-quickstart)

Ubuntu

```
sudo apt install nginx
sudo systemctl status nginx
```

---

### 🐌 Set up Domain Name for SSL

```
sudo vim /etc/nginx/sites-available/default
```

Configure server name

```
server {
        listen 80;
        listen [::]:80;

        server_name {DOMAIN_NAME};

        root /var/www/html;
        index index.html;
}
```

---

### 🐭 Certbot

> Using Certbot for generate SSL keys for using HTTPS.

[Certbot Installation](https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx)

Ubuntu

```
sudo snap install core; sudo snap refresh core
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo certbot certonly --nginx
```

🚀 Then... location of certificates in /etc/letsencrypt/live/{{DOMAIN_NAME}}/

---

### ⚠️ !! IF YOU WANT TO UNINSTALL NGINX

```
sudo systemctl stop nginx

sudo systemctl status nginx

sudo apt-get purge nginx nginx-common
```

---

### 👽 Docker-monitoring-bundle

    git clone https://github.com/Winz-Mawin7/docker-monitoring-bundle.git ~/docker-monitoring-bundle

    cd docker-monitoring-bundle

---

### 🐙 Docker Swarm

```
sudo docker swarm init
```

---

### Kong & Konga 🦍

> Kong is an API Gateway and have many plugins...

> Konga is an open-source for Kong GUI.

```
sudo sh -c 'cd Kong && docker-compose up -d'
```

- You can change ENV variables

  - KONG_PG_DATABASE
  - KONG_PG_USER
  - KONG_PG_PASSWORD

  _*Default is "kong"*_

🤔 Wait until Kong start...

---

### Services 🐯

In this repository I used my services in Docker hub for **FCM** messaging API you should <br />

edit docker-compose.yml in Services folder to match your services do you want.

_*I used Express Prom Bundle to Export metrics*_

```
sudo sh -c 'cd Services && docker-compose up -d'
```

---

**Configure Kong Services** 🙊

- **Configure via Konga GUI: PORT 1337**, http://localhost:1337

  - Add Services
  - Add Routes
  - Add Certificate

- **Config via REST API**

**Add Services**

```
curl -i -X POST http://localhost:8001/services/ \
--data 'name={SERVICE_NAME}' \
--data 'url=http://{SERVICE_URI}:{PORT}'
```

**Add Routes**

```
curl -i -X POST http://localhost:8001/services/{SERVICE_NAME}/routes/ \
--data 'name={ROUTE_NAME}' \
--data 'paths[]=/'
```

**Add Certificate by curl to API**

```
curl -i -m 60 -X POST http://localhost:8001/certificates  \
-F "cert=$(sudo cat /etc/letsencrypt/live/{DOMAIN_NAME}/cert.pem)" \
-F "key=$(sudo cat /etc/letsencrypt/live/{DOMAIN_NAME}/privkey.pem)" \
-F "snis=['{DOMAIN_NAME}']"
```

---

### Monitoring 📈

#### Prometheus & Exporter & Grafana

> _**Prometheus**_ is an open-source time series database for monitoring system using promQL.

> _**Exporter**_ is an export metrics for giving prometheus scrape.

> _**Grafana**_ is an open-source for monitoring dashboard can add various data sources.

Using below command to copy cert and key for grafana because grafana not have permission for access /etc/letsencrypt/live folder

```
sudo cat /etc/letsencrypt/live/{DOMAIN_NAME}/cert.pem >> ~/docker-monitoring-bundle/Monitoring/certs/cert.pem

sudo cat /etc/letsencrypt/live/{DOMAIN_NAME}/privkey.pem >> ~/docker-monitoring-bundle/Monitoring/certs/privkey.pem

sudo sh -c 'cd Monitoring && docker-compose up -d'
```

---

### 😻 Finally

- Grafana https://localhost:3000

  - Add Data Source Prometheus.
    - URL: prometheus:9090
  - Import JSON file from folder Monitoring/Dashboard.json

---

### 😎 Bonus

If setup already done you can run all docker-compose by.

```
sudo sh -c 'cd Kong && docker-compose up -d'

sudo sh -c 'cd Monitoring && docker-compose up -d'

sudo sh -c 'cd Services && docker-compose up -d'
```

- If you want to down all docker-compose `option -v` for remove volume

```
sudo sh -c 'cd Kong && docker-compose down'

sudo sh -c 'cd Monitoring && docker-compose down'

sudo sh -c 'cd Services && docker-compose down'
```

---

### PORTS (Open World)

- 3000 : Grafana
- 5432 : Postgresql
- 8000 : Kong HTTP
- 8088: Node-FCM
- 8443 : Kong HTTPS
- 9090 : Prometheus
- 9100 : Node_Exporter

### PORTS (Not Open World)

- 8001 : Kong Admin API
