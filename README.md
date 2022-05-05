# Hosting multiple sites with traefik with SSL

Template at : [github.com/alexiscotel/docker-traefik-multisites](https://github.com/alexiscotel/docker-traefik-multisites)

## commands
### start Traefik container
use for http
```
docker-compose -f docker-compose.yml up -d
```
use for http & https
```
docker-compose -f docker-compose.ssl.yml up -d
```
### start sites containers cluster
For each sites you want to connect (site1, site2), add a __traefik__ network to service that contains sources code.<br>
Add it 1st so traefik performs better
```
networks:
    - traefik
```
and add labels
```
labels:
    - "traefik.enable=true"
    # http
    - "traefik.http.routers.http_site1_com.rule=Host(`site1.com`)"
    - "traefik.http.routers.http_site1_com.entrypoints=http"
    # https
    - "traefik.http.routers.https_site1_com.rule=Host(`site1.com`)"
    - "traefik.http.routers.https_site1_com.entrypoints=https"
    - "traefik.http.routers.https_site1_com.tls=true"
    - "traefik.http.routers.https_site1_com.tls.certresolver=myresolver"
```

## Information
* Change email in `docker-compose.ssl.yml` line 14
* Create file `acme.json` then change permission `chmod 600 acme.json` (use file properties for windows)

Don't forget to edit hosts file in dev mode
```
127.0.0.1 site1.com
```

## Website access

the website are accessible at the addresses
```
https://site1.com
```
Don't forget to add the __traefik__ network in networks sections
```
networks:
    # other containers networks
    traefik:
        name: traefik_global
```

## Javier Lopez tutorials
* http: [Host several sites in a single box with docker and traefik v2, http](http://javier.io/blog/en/2020/12/01/host-several-sites-in-a-single-box-with-docker-and-traefik-http.html)
* https: [Host several sites in a single box with docker and traefik v2, https](http://javier.io/blog/en/2020/12/01/host-several-sites-in-a-single-box-with-docker-and-traefik-https.html)