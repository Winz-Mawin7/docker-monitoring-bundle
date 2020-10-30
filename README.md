### Generate Certificate for HTTPS

    - Install NGINX
    - Install CERTBOT

- [link to NGINX Instructions!](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04-quickstart)
- [link to Certbot Instructions!](https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx)

After generated Certbot file inside

- /etc/letsencrypt/live/{WEBSITE_DOMAIN}/

!! CERTIFICATE used in Kong & Grafana

```
sudo sh -c 'cd Kong && docker-compose up'

sudo sh -c 'cd Monitoring && docker-compose up'

sudo sh -c 'cd Services && docker-compose up'
```
