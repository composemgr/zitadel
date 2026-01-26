## 👋 Welcome to zitadel 🚀

Identity infrastructure with authentication and authorization

## 📋 Description

Identity infrastructure with authentication and authorization

## 🚀 Services

- **server**: ghcr.io/zitadel/zitadel:latest

### Infrastructure Components

- **db**: Postgres database


## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/zitadel/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/zitadel" ~/.local/srv/docker/zitadel
cd ~/.local/srv/docker/zitadel
docker compose up -d
```

### Option 3: Using composemgr
```bash
composemgr install zitadel
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
APP_ADMIN_USER=administrator
APP_TEMP_PASS=lkdfjdlkjaflkjadlgknvczmbnvnoi4e
```

See `docker-compose.yaml` for complete list of configurable options.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:59048

## 📂 Volumes

- `./rootfs/config/zitadel/machinekey` - Data storage
- `./rootfs/data/db/postgres` - Data storage

## 🔐 Security

- Change all default passwords before deploying to production
- Use strong secrets for all authentication tokens
- Configure HTTPS using a reverse proxy (nginx, traefik, caddy)
- Regularly update Docker images for security patches
- Backup your data regularly

## 🔍 Logging

```shell
docker compose logs -f server
```

## 🛠️ Management

```bash
# Start services
docker compose up -d

# Stop services
docker compose down

# Update to latest images
docker compose pull && docker compose up -d

# View logs
docker compose logs -f

# Restart services
docker compose restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
