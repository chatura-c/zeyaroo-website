# zeyaroo-website

Static landing page for [Zeyaroo](https://zeyaroo.com) — a creator-client media collaboration platform.

## Structure

```
zeyaroo-website/
├── brochure.html        # Main landing page (deployed as index.html)
├── assets/
│   ├── logo.svg
│   ├── logo-dark.svg
│   ├── favicon.svg
│   ├── apple-touch-icon.png
│   └── og-image.png
├── deploy.sh.example    # Deploy script template (copy → deploy.sh)
└── deploy.sh            # Your local deploy script (gitignored)
```

## Deployment

The landing page is deployed via rsync over SSH to the production server at `/opt/zeyaroo/landing/`. Caddy serves the directory directly.

### Setup

```bash
cp deploy.sh.example deploy.sh
# Edit deploy.sh — replace YOUR_SERVER_IP with the actual server IP
chmod +x deploy.sh
```

### Deploy

```bash
./deploy.sh
```

This syncs `brochure.html` → `index.html` and the `assets/` directory to the server.

### Manual deploy (without script)

```bash
SERVER="ubuntu@YOUR_SERVER_IP"
ssh $SERVER "mkdir -p /opt/zeyaroo/landing/assets"
rsync -az brochure.html $SERVER:/opt/zeyaroo/landing/index.html
rsync -az assets/ $SERVER:/opt/zeyaroo/landing/assets/
```

## Caddy config

Caddy on the prod VM serves the landing under the `zeyaroo.com` domain. The relevant snippet in the Caddy config:

```
zeyaroo.com {
    root * /opt/zeyaroo/landing
    file_server
}
```
