# Serve this documentation

?> You have to clone this project first. Use top right corner for that.

You can serve this documentation using Docker.

## With distant access available

Go into the main directory of this project, then run:

```bash
docker run --rm -ti -v $PWD:/local -v ~/.ngrok2:/root/.ngrok2 monsieurbiz/nginx-ngrok
```

## For local use only

Go into the main directory of this project, then run:

```bash
docker run --rm -ti -p 8080:80 -v $PWD:/usr/share/nginx/html:rw nginx
```

Then go to <http://localhost:8080>.