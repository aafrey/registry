## Registry setup
Provide the following updates to the services listed in the `docker-compose.yml`
file.


Update `traefik` acme.domains, acme.email command flag w/ your domain information
```
--acme.domains='yourdomain.co,www.yourdomain.co'
--acme.email=youremail@gmail.com
```

Update `minio` env vars. These can be anything
```
MINIO_ACCESS_KEY: YOURMINIOACCESSKEY
MINIO_SECRET_KEY: YOURMINIOSECRETKEY
```

Update `minio` deploy label to include your domain
```
- traefik.frontend.rule=Host:yourdomain.co
```

Update `registry` deploy label to include the domain name to which images
will be pushed
```
- traefik.frontend.rule=Host:registry.yourdomain.co
```

In the `config.yml` file, update the following information
```
accesskey: YOURMINIOACCESSKEY
secretkey: YOURMINIOSECRETKEY
regionendpoint: https://yourdomain.com
```

## Launch services
```
docker swarm init
docker deploy stack -c docker-compose.yml registry
```

Login to Minio at https://yourdomain.com and create a bucket called `docker`

Tag an image and push to the registry
```
docker tag traefik registry.mydomain.co/traefik
docker push registry.mydomain.co/traefik
```
