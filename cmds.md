This contains a list commands used to run this project

Building the docker image
```
docker build -t polls .
```

Docker images
```
docker images
```

Running migrations

```
docker run --env-file .env django-polls sh -c "python manage.py makemigrations && python manage.py migrate"
```

Running the shell
```
docker run -it --env-file .env django-polls sh
```

Collect static

```
docker run --env-file .env django-polls sh -c "python manage.py collectstatic --noinput"
```

Running the container

```
cd docker-ngnix-kubernetes/
docker run --env-file .env -p 80:8000 polls:v0
```

running the nginx container
The -v flag mounts the config file into the Nginx container

```
docker run --rm --name nginx -p 80:80 -p 443:443 \
    -v ~/conf/nginx.conf:/etc/nginx/conf.d/nginx.conf:ro \
    -v /etc/letsencrypt:/etc/letsencrypt \
    -v /var/lib/letsencrypt:/var/lib/letsencrypt \
    -v /var/www/html:/var/www/html \
    nginx:1.19.0
```



Dockerizing the certbot certificate
Staging
```
docker run -it --rm -p 80:80 --name certbot \
         -v "/etc/letsencrypt:/etc/letsencrypt" \
         -v "/var/lib/letsencrypt:/var/lib/letsencrypt" \
         certbot/certbot certonly --standalone --staging -d splashco.in
```
Production
```
docker run -it --rm -p 80:80 --name certbot \
         -v "/etc/letsencrypt:/etc/letsencrypt" \
         -v "/var/lib/letsencrypt:/var/lib/letsencrypt" \
         certbot/certbot certonly --standalone -d splashco.in
```