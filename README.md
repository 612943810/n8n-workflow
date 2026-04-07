# n8n Workflow Environment

A Docker Compose setup for running n8n, a powerful workflow automation tool, with nginx reverse proxy for local development and testing of AI workflows.

## Features

- **n8n**: Open-source workflow automation tool
- **SQLite Database**: Lightweight, file-based database for data persistence
- **Nginx Proxy**: Clean URLs and reverse proxy setup
- **Custom Domain**: Optional fake domain (`n8n-workflow.local`) for development

## Prerequisites

- Docker (version 20.10 or later)
- Docker Compose (version 1.29 or later)

## Quick Start

1. **Clone or navigate to this repository**
   ```bash
   cd /path/to/n8n-workflow
   ```

2. **Configure environment variables**
   ```bash
   cp .env.example .env  # Copy the template
   # Edit .env with your preferred credentials
   ```

3. **Start the services**
   ```bash
   docker-compose up -d
   ```

4. **(Optional) Set up custom domain**
   Add to your `/etc/hosts` file:
   ```bash
   sudo nano /etc/hosts
   ```
   Add this line:
   ```
   127.0.0.1 n8n-workflow.local
   ```

5. **Access n8n**
   - Via nginx proxy: `http://n8n-workflow.local` or `http://localhost`
   - Direct access: `http://localhost:5678`

6. **Log in**
   - Check your `.env` file for the username and password you configured
   - ⚠️ **Important**: Never use default credentials in production!

## Services

| Service | Description | Port |
|---------|-------------|------|
| **n8n** | Workflow automation platform with web UI | 5678 |
| **nginx** | Reverse proxy server | 80 |

## Volumes

- `n8n_data`: Stores n8n configuration, workflows, and SQLite database

## Configuration

### Environment Variables

Sensitive configuration is stored in the `.env` file (automatically loaded by Docker Compose). The repository includes a template `.env.example` file with placeholder values.

**Important**: Never commit your actual `.env` file to version control - it's already in `.gitignore`.

To customize:
1. Copy the template: `cp .env.example .env`
2. Edit `.env` with your values

Available variables:
```bash
# Basic Authentication
N8N_BASIC_AUTH_ACTIVE=true          # Enable/disable basic auth
N8N_BASIC_AUTH_USER=your_username   # Your username
N8N_BASIC_AUTH_PASSWORD=your_password # Your password (CHANGE THIS!)

# Database Configuration (SQLite by default)
# Uncomment for PostgreSQL:
# DB_TYPE=postgresdb
# DB_POSTGRESDB_HOST=postgres
# DB_POSTGRESDB_PORT=5432
# DB_POSTGRESDB_DATABASE=n8n
# DB_POSTGRESDB_USER=n8n
# DB_POSTGRESDB_PASSWORD=your_secure_password
```

### Nginx Configuration

The nginx configuration is in `nginx.conf`. It includes:
- Reverse proxy to n8n service
- Custom domain support (`n8n-workflow.local`)
- Fallback for localhost access

## Management Commands

```bash
# Start services in background
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down

# Rebuild and restart
docker-compose up -d --build

# View running containers
docker-compose ps
```

## Security Notes

⚠️ **Critical Security Information**

This setup includes default credentials for development only. **Never use default passwords in production!**

**Security Best Practices:**
- ✅ Environment variables are now stored in `.env` file (not committed to git)
- ✅ `.env` is in `.gitignore` to prevent accidental commits
- ✅ Create your own `.env` file with unique credentials
- ❌ Never commit actual `.env` files to version control
- ❌ Never share `.env` files or their contents

**Before going live:**
- Change default username/password in your `.env` file
- Use strong, unique passwords (at least 12 characters)
- Consider using external authentication (OAuth, LDAP)
- Add SSL/TLS certificates for HTTPS
- Review n8n security documentation

## Troubleshooting

### Common Issues

1. **Port 80 already in use**
   - Stop other services using port 80
   - Or modify nginx port in `docker-compose.yml`

2. **Permission denied on /etc/hosts**
   - Use `sudo` when editing hosts file

3. **n8n not accessible**
   - Check if services are running: `docker-compose ps`
   - View logs: `docker-compose logs n8n`

4. **Database issues**
   - Data is stored in Docker volume `n8n_data`
   - To reset: `docker-compose down -v` (⚠️ This deletes all data)

### Useful Commands

```bash
# Check service health
docker-compose ps

# View specific service logs
docker-compose logs n8n
docker-compose logs nginx

# Restart specific service
docker-compose restart n8n

# Clean up (removes containers and networks)
docker-compose down

# Clean up including volumes (⚠️ deletes data)
docker-compose down -v
```

## Development

This setup is optimized for local development and testing of n8n workflows. For production deployments, consider:

- Using managed databases (PostgreSQL, MySQL)
- Implementing proper SSL certificates
- Setting up monitoring and logging
- Using environment-specific configurations

## License

This project is for personal workflow testing and development purposes.
