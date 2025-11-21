# Site Setup Guide

This guide explains how to set up the `xpert_lora.my` site with the `xpert_lora_app` custom app.

## Prerequisites

- Docker and Docker Compose installed
- frappe_docker repository cloned and running

## Site Information

- **Site Name**: `xpert_lora.my`
- **Installed Apps**:
  - frappe (15.89.0)
  - erpnext (15.89.0)
  - xpert_lora_app (0.0.1)

## Setting Up the Site

### 1. Start the containers

```bash
docker compose up -d
```

### 2. Create the site (if not already created)

```bash
docker compose exec backend bench new-site xpert_lora.my --mariadb-user-host-login-scope='172.%.%.%'
```

### 3. Install apps to the site

```bash
# Install ERPNext
docker compose exec backend bench --site xpert_lora.my install-app erpnext

# Install custom app
docker compose exec backend bench --site xpert_lora.my install-app xpert_lora_app
```

### 4. Access the site

- **URL**: http://localhost:8080
- **Site**: Select `xpert_lora.my` from the site selector

## Custom App Development

The custom app `xpert_lora_app` is located at:
- **In Container**: `/home/frappe/frappe-bench/apps/xpert_lora_app`
- **Local Copy**: `apps/xpert_lora_app/`

### Making Changes

1. Edit files in `apps/xpert_lora_app/`
2. Copy changes to container (or use volume mount)
3. Rebuild assets: `docker compose exec backend bench build --app xpert_lora_app`
4. Restart services if needed: `docker compose restart backend`

## Backup and Restore

### Backup Site

```bash
docker compose exec backend bench --site xpert_lora.my backup --with-files
```

### Restore Site

```bash
docker compose exec backend bench --site xpert_lora.my restore /path/to/backup.tar.gz
```

## Notes

- Site data and configuration are stored in Docker volumes (not in git)
- Database credentials are in `sites/xpert_lora.my/site_config.json` (excluded from git)
- For production, ensure proper security measures and environment variables

