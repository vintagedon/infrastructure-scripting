# Gogs Docker Setup with SQLite3

This project contains a Docker Compose setup for Gogs, a self-hosted Git service, using SQLite3 as the database backend. This setup includes basic and advanced configuration options suitable for home lab use.

Gogs is a painless self-hosted Git service written in Go. It aims to be the easiest, fastest, and most painless way of setting up a self-hosted Git service. With Gogs, you can easily set up your own GitHub-like Git server.

**Disclaimer:** This setup is based on general best practices and may not reflect the most current configuration options or requirements for Gogs. Always refer to the [official Gogs documentation](https://gogs.io/docs) for the most up-to-date and accurate information.

## GitHub Repository

https://github.com/vintagedon/docker-compose-cookbook/development-ci-cd/gogs-sqlite

## Prerequisites

- Docker
- Docker Compose
- Basic understanding of Git and version control systems

## Project Structure

```
.
├── docker-compose.yml
├── .env
└── README.md
```

## Files
- `docker-compose.yml`: Gogs service configuration
- `.env`: Environment variables for Gogs configuration
- `README.md`: Project information and instructions

## Usage

1. Edit the `.env` file with your desired configuration.
2. Run the container:

```bash
docker-compose up -d
```

3. Access Gogs at `http://your-server-ip:3000`. On first run, you'll be redirected to the installation page.

4. Check the logs to ensure Gogs is running correctly:

```bash
docker-compose logs gogs
```

You should see output similar to:

```
2023/07/27 12:00:00 [INFO] Gogs 0.12.3
2023/07/27 12:00:00 [INFO] Cache Service Enabled
2023/07/27 12:00:00 [INFO] Session Service Enabled
2023/07/27 12:00:00 [INFO] SQLite3 Supported
2023/07/27 12:00:00 [INFO] Run Mode: Production
2023/07/27 12:00:00 [INFO] Listen: http://0.0.0.0:3000
```

## Expected Output

When Gogs is running correctly, you should see:

1. The Gogs container running without any error messages in the logs.
2. The Gogs web interface accessible at `http://your-server-ip:3000`.
3. Ability to create users, repositories, and perform Git operations.
4. The output of `docker ps` should show the Gogs container running. For example:

   ```
   $ docker ps
   CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                    NAMES
   abc123def456   gogs/gogs:latest      "/app/gogs/docker/s…"   10 minutes ago   Up 10 minutes   0.0.0.0:3000->3000/tcp   gogs
   ```

## Configuration Options

### Basic Options

- `GOGS_PORT`: The external port on which Gogs will be accessible (default: 3000)
- `GOGS_CONTAINER_NAME`: The name of the Docker container (default: gogs)

### Common Options

- `GOGS_APP_NAME`: The name of your Gogs instance (default: Gogs)
- `GOGS_RUN_MODE`: The run mode for Gogs (default: prod)
- `GOGS_DOMAIN`: The domain name of your Gogs instance (default: localhost)
- `GOGS_SSH_PORT`: The SSH port for Git operations (default: 22)

### Advanced Options

- `GOGS_SECRET_KEY`: A secret key for Gogs internal usage. Should be a long random string.
- `GOGS_DISABLE_REGISTRATION`: Set to true to disable self-registration (default: false)
- `GOGS_REQUIRE_SIGNIN_VIEW`: Set to true to require signin to view any page (default: false)

Usage:

1. Uncomment relevant lines in the `.env` file.
2. Set desired values.
3. Restart the container:

```bash
docker-compose down && docker-compose up -d
```

## Data Persistence

Gogs data is stored in Docker volumes. You can choose between anonymous volumes (default) or named volumes:

### Anonymous Volumes (default):
- volume1: Stores Gogs data, including repositories and SQLite database
- volume2: Stores Gogs configuration files

### Named Volumes:
If you set `USE_NAMED_VOLUMES=true` in the `.env` file, the following named volumes will be used:
- gogs_data: Stores Gogs data, including repositories and SQLite database
- gogs_config: Stores Gogs configuration files

These volumes ensure your data persists across container restarts and updates. Named volumes make it easier to backup and manage your data.

## Networking

Gogs uses the default bridge network created by Docker Compose. This network, named `gogs_network`, allows the Gogs container to communicate with other services if needed.

## Security Note

1. Always use strong passwords for your Gogs accounts, especially the admin account.
2. Consider using HTTPS by setting up a reverse proxy (like Nginx) in front of Gogs.
3. Regularly update your Gogs instance to the latest version to get security fixes.
4. Be cautious when exposing the SSH port (22) to the internet. Consider using a non-standard port for SSH.
5. Enable two-factor authentication for additional security.

## Troubleshooting

Check logs: `docker-compose logs gogs`

Common issues:

1. Can't access Gogs web interface: Ensure the correct port is exposed in `docker-compose.yml` and not blocked by your firewall.
2. Git push/pull fails: Check if the SSH port is correctly configured and accessible.
3. Database errors: Ensure the SQLite database file has the correct permissions.

For more detailed troubleshooting, please refer to the [official Gogs documentation](https://gogs.io/docs/intro/troubleshooting).

## Contributing

Feel free to submit issues, fork the repository and send pull requests!

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.