## 👋 Welcome to keycloak 🚀

Open-source identity and access management

## 📋 Description

Keycloak is an open-source identity and access management solution for
applications. It provides single sign-on, identity brokering and user
management, backed by a Postgres database.

## 🚀 Services

- **app**: quay.io/keycloak/keycloak:latest

### Infrastructure Components

- **db**: Postgres database

## 📦 Installation

### Option 1: Quick Install

```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/keycloak/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone

```bash
git clone "https://github.com/composemgr/keycloak" ~/.local/srv/docker/keycloak
cd ~/.local/srv/docker/keycloak
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr

```bash
composemgr install keycloak
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
APP_ADMIN_USER=kcadmin
APP_ADMIN_PASS=replace-with-a-random-password
DB_USER_NAME=keycloak
DB_USER_PASS=replace-with-a-random-password
DB_CREATE_DATABASE_NAME=keycloak
```

See `docker-compose.yaml` for complete list of configurable options.

### Environment Files

`default.env.sample` holds the full set of variables and is copied to `.env`
before starting the stack:

```bash
cp default.env .env
docker compose --env-file .env up -d
```

`app.env.sample` holds the app-specific overrides and is copied to `app.env`:

```bash
cp app.env.sample app.env
```

`composemgr up` applies `app.env` and `default.env` automatically. Raw
`docker compose` commands need `--env-file .env` to pick up the same values.

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, published on `172.17.0.1:64223` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

The proxy variants publish no host ports. The app is reachable only through the
proxy network, and the Postgres service stays internal to the project network in
every variant.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64223

The Keycloak console redirects to the admin console at `/admin`. Set
`BASE_HOST_NAME` to the public hostname before starting the stack so generated
links and redirects resolve correctly.

## 📂 Volumes

- `./volumes/db/postgres/keycloak` - Postgres database data

## 🔐 Security

- Change the bootstrap admin password before deploying to production
- Use strong secrets for all authentication tokens
- Configure HTTPS using a reverse proxy (nginx, traefik, caddy)
- Regularly update Docker images for security patches
- Backup your data regularly

The `KC_BOOTSTRAP_ADMIN_*` variables only apply on the very first start, when the
admin account is created. Change the password in the Keycloak console
afterwards.

## 🔍 Logging

```shell
docker compose --env-file .env logs -f app
```

## 🛠️ Management

```bash
# Start services
docker compose --env-file .env up -d

# Stop services
docker compose --env-file .env down

# Update to latest images
docker compose --env-file .env pull && docker compose --env-file .env up -d

# View logs
docker compose --env-file .env logs -f

# Restart services
docker compose --env-file .env restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
