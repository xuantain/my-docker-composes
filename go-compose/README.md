# go-compose


## Dependencies

Make sure you have all these required dependencies installed on your host machine.

### MacOS
```bash
# System dependencies
brew install wget mkcert nss
```

### Linux
```bash
# System dependencies
sudo apt install wget libnss3-tools \ 
  && curl -JLO "https://dl.filippo.io/mkcert/latest?for=linux/amd64"
  && chmod +x mkcert-v*-linux-amd64 \
  && sudo cp mkcert-v*-linux-amd64 /usr/local/bin/mkcert
```

## Docker

Copy `docker-compose.override.yml.dist` to `docker-compose.override.yml`

```bash
cp docker-compose.override.yml.dist docker-compose.override.yml
```

Edit `docker-compose.override.yml` with your preferences.

## Installation HTTPS

There are two ways to access to the website: either through https://localhost or https://todo.go.com

You must have `mkcert` installed to generate ssl certification keys for localhost.

```bash
./nginx/self-cert
```

## Start and Stop the docker-compose

To **Build** and **Start**
```bash
docker-compose up --build
```

To **Start**
```bash
docker-compose up
```

To **Stop** `Ctrl + C`


## Installation DNS

Add this to `/etc/hosts`
```bash
127.0.0.1  your-custom-domain
```

Update `docker-compose.override.yml`
```bash
services:
  backend:
    hostname: your-custom-domain
    networks:
      app-network:
        aliases:
          - your-custom-domain
```