# n8n-workflow

This container will be my workflow testing environment for AI workflows.

## Setup

1. Ensure you have Docker and Docker Compose installed on your system.

2. Clone or navigate to this repository.

3. Run the following command to start the n8n service:

   ```bash
   docker-compose up -d
   ```

4. (Optional) For a custom domain, add the following line to your `/etc/hosts` file:
   ```
   127.0.0.1 n8n-workflow.local
   ```
   You can do this with: `sudo nano /etc/hosts`

5. Open your browser and go to `http://n8n-workflow.local` (via nginx proxy) or `http://localhost:5678` (direct to n8n)

5. Log in with the default credentials:
   - Username: `admin`
   - Password: `password`

## Services

- **n8n**: Workflow automation tool running on port 5678 with SQLite database
- **nginx**: Reverse proxy server running on port 80, forwarding requests to n8n

## Volumes

- `n8n_data`: Persists n8n configuration, workflows, and SQLite database

## Stopping the Services

To stop the services, run:

```bash
docker-compose down
```

## Environment Variables

You can customize the setup by modifying the environment variables in `docker-compose.yml`. For security, consider changing the default basic auth credentials and database password in a production environment.
