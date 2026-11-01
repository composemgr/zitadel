## 👋 Welcome to zitadel 🚀

Identity infrastructure with authentication and authorization

## 📋 Description

Zitadel is an identity provider that issues OIDC, OAuth2, and SAML tokens and
serves a login UI on the same origin. This stack runs a single Zitadel
container with `start-from-init`, backed by PostgreSQL, behind the host nginx
proxy. The machine key is persisted so encrypted secrets survive a rebuild.

## 🚀 Services

- **app**: ghcr.io/zitadel/zitadel:latest

### Infrastructure Components

- **zitadel-db**: Postgres database

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/zitadel/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/zitadel" ~/.local/srv/docker/zitadel
cd ~/.local/srv/docker/zitadel
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr
```bash
composemgr install zitadel
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
DB_CREATE_DATABASE_NAME=zitadel
DB_USER_NAME=zitadel
DB_USER_PASS=replace-with-a-random-password
DB_ADMIN_NAME=postgres
DB_ADMIN_PASS=replace-with-a-random-password
APP_SECRET_KEY=replace-with-a-random-32-byte-hex-string
APP_ADMIN_USER=administrator
APP_ADMIN_PASS=replace-with-a-random-password
ZITADEL_EXTERNAL_PORT=443
ZITADEL_EXTERNAL_SECURE=true
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
| `docker-compose.yaml` | Default, published on `172.17.0.1:64398` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

Only the `app` service carries the proxy labels and joins the external
network. The database stays on the internal project network. Zitadel runs
with `--tlsMode external`, so it expects the proxy to terminate TLS, keep
`ZITADEL_EXTERNAL_SECURE` set to `true` and `ZITADEL_EXTERNAL_PORT` at `443`
for any hostname that resolves publicly.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64398
- **OIDC Discovery**: http://172.17.0.1:64398/.well-known/openid-configuration

`ZITADEL_EXTERNALDOMAIN` drives every issuer URL Zitadel hands to clients. If
you reach the stack by IP on the published port, the discovery document will
still advertise `BASE_HOST_NAME`, so use a resolvable hostname for anything
beyond a quick look.

## 📂 Volumes

- `./volumes/config/zitadel/machinekey` - Machine key, required to decrypt
  stored secrets
- `./volumes/db/postgres/zitadel` - Database data

## 🔐 Security

- `APP_SECRET_KEY` is the master key and must be exactly 32 characters, it
  encrypts every secret Zitadel stores
- Changing the master key after the first start makes all existing encrypted
  values unreadable
- Never lose `./volumes/config/zitadel/machinekey`, without it a restore
  cannot decrypt the database
- Change `APP_ADMIN_PASS` before the first start, `start-from-init` only
  creates that account when the instance is empty
- Set `ZITADEL_EXTERNAL_SECURE` to `false` if you deliberately serve the
  login over plain HTTP, tokens would otherwise be sent in clear
- Use HTTPS via the reverse proxy in production
- The database is reachable only on the internal project network
- Regularly update Docker images for security patches
- Backup both volumes regularly

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
