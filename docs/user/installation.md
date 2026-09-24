# Installation

SyncEngine offers multiple installation options depending on your environment and use case.  
You can install it using **Docker**, the **official Installer script**, or **Composer**.  
Each method is designed to make the setup process as smooth and reliable as possible.

---

## Option 1: Install via Docker

The **Docker image** provides a complete, preconfigured environment to run SyncEngine with zero manual setup.  
This is the fastest way to start experimenting locally or in a development environment.

> **Repository:** [github.com/SyncEngine/docker](https://github.com/SyncEngine/docker)

### Docker Requirements

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

### Installation Steps

1. **Clone the Docker repository:**

   ```bash
   git clone https://github.com/SyncEngine/docker.git
   cd docker
   ```

2. **Start the environment:**

   ```bash
   docker compose build
   docker compose up -d
   ```

3. **Access SyncEngine:**

   Once the containers are running, open your browser and visit:

   ```
   http://localhost
   ```

   You’ll see the SyncEngine setup screen, where you can configure your instance for the first time.

### Why use Docker?

- No dependency issues — everything comes preconfigured.
- Quick local development setup.
- Easily reproducible across environments.

---

## Option 2: Install via the Installer Script

The **SyncEngine Installer** automates a complete production-ready deployment.  
It installs all dependencies, configures your environment, and prepares your server to run SyncEngine seamlessly.

> **Repository:** [github.com/SyncEngine/installer](https://github.com/SyncEngine/installer)

### Install Requirements

All [System Requirements (see below)](#system-requirements)

### Installation Steps

1. **Place the script**:  
   Copy `install.php` into your project’s `/public` directory.

2. **Open in browser**:  
   Navigate to the script in your browser, e.g.:
   ```
   http://localhost/your-project/public/install.php
   ```
3. **Follow the prompts:**

   The installer will guide you through configuration steps, including environment setup, database connection, and service initialization.

After completion, you’ll be able to access your SyncEngine installation from your configured domain or IP.

### Why use the Installer?

- Perfect for **live servers** and production environments.
- Automatically configures PHP, environment variables, and dependencies.
- Simple guided setup.
- Easy to setup next to your existing site.

---

## Option 3: Install via Composer

If you prefer a manual installation or plan to integrate SyncEngine into an existing Symfony project, you can install it via **Composer**.

> This method is ideal for advanced users and developers.

### Requirements

- Composer
- All [System Requirements (see below)](#system-requirements)


## Which Method Should You Choose?

| Environment | Recommended Method | Notes |
|--------------|--------------------|-------|
| Local development | Docker | Quickest and most reliable for development |
| Production server | Installer Script | Handles environment and dependencies automatically |
| Custom integration | Composer | Ideal for developers integrating SyncEngine into an existing project |

## System Requirements
> Full version requirements: [github.com/SyncEngine/SyncEngine/blob/master/requirements.json](https://github.com/SyncEngine/SyncEngine/blob/master/requirements.json)

### Webserver
- A configured web server (Apache, Nginx, Caddy, etc.)
- Database: SQLite, MySQL/MariaDB, PostgreSQL or other supported by Doctrine v3
- Filesystem access
- Docroot set to the `/public` directory of your SyncEngine installation
- Rewrite enabled on your web server

### PHP
- PHP version compatible with current Symfony LTS (8.2 or higher > Will jump to 8.4 soon)
- Minimal PHP extensions required by Symfony and Doctrine, SyncEngine does not enforce additional extensions, but some may be required depending on your configuration and usage.

#### PHP INI
- `memory_limit` should be set to at least 256M. For longer running tasks, consider increasing this limit based on your requirements.
- `max_execution_time` should be set to a reasonable value (e.g., 30 seconds) to allow for longer-running tasks.

> You are likely running automations through automated processes or cron jobs, which may require longer execution times and higher memory limits.
> 1. You may want to set `max_execution_time` to 0 (unlimited) in your CLI PHP configuration.
> 2. You may want to set `memory_limit` to a higher value in your CLI PHP configuration, especially if you are processing large datasets.

### Resources
- Enough disk space and memory to store your data. Traces can grow large depending on your configuration and usage.
- Enough CPU and RAM resources available for the application to run efficiently, especially if you plan to handle large volumes of data or complex workflows.

---

## Next Steps

Once SyncEngine is installed, continue with:

- [Setup Guide](setup.md)
- [Dashboard Overview](dashboard.md)
- [Tasks Reference](tasks.md)

---

> **Tip:** If you’re unsure which installation method to use, start with **Docker**.  
> It’s the easiest and fastest way to get SyncEngine up and running locally.
