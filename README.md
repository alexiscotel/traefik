# Hosting multiple sites with traefik with SSL

Template at : [https://github.com/alexiscotel/looking4flat](https://github.com/alexiscotel/docker-traefik-multisites)

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
next, for each sites you want to connect, add a `traefik` network to service that contains sources code
```
networks:
    - traefik #add 1st so traefik performs better
```
and add labels
```
labels:
    - "traefik.enable=true"
    # http
    - "traefik.http.routers.http_looking4flat.rule=Host(`looking4flat.com`)"
    - "traefik.http.routers.http_looking4flat.entrypoints=http"
    # https
    - "traefik.http.routers.https_looking4flat.rule=Host(`looking4flat.com`)"
    - "traefik.http.routers.https_looking4flat.entrypoints=https"
    - "traefik.http.routers.https_looking4flat.tls=true"
    - "traefik.http.routers.https_looking4flat.tls.certresolver=myresolver"
```

## Information
* Change email in `traefik/docker-compose.ssl.yml` line 14
* Create file `acme.json` then change permission `chmod 600 acme.json` (use file properties for windows)

Don't forget to edit hosts file in dev mode
```
127.0.0.1 site1.com
127.0.0.1 site2.com
```

## Websites access

the websites are accessible at the addresses
```
https://site1.com
https://site2.com
```
Don't forget to add the `traefik` network in networks sections
```
networks:
    # containers networks
    traefik:
        name: traefik_global
```

## Javier Lopez tutorials
* http: [Host several sites in a single box with docker and traefik v2, http](http://javier.io/blog/en/2020/12/01/host-several-sites-in-a-single-box-with-docker-and-traefik-http.html)
* https: [Host several sites in a single box with docker and traefik v2, https](http://javier.io/blog/en/2020/12/01/host-several-sites-in-a-single-box-with-docker-and-traefik-https.html)