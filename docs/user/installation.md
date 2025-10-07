# Installation

SyncEngine offers multiple installation options depending on your environment and use case.  
You can install it using **Docker**, the **official Installer script**, or **Composer**.  
Each method is designed to make the setup process as smooth and reliable as possible.

---

## Option 1: Install via Docker

The **Docker image** provides a complete, preconfigured environment to run SyncEngine with zero manual setup.  
This is the fastest way to start experimenting locally or in a development environment.

> **Repository:** [github.com/SyncEngine/docker](https://github.com/SyncEngine/docker)

### Requirements

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

### Requirements

- PHP 8.3 or higher
- Database credentials (MySQL, PostgreSQL, or SQLite)
- Filesystem access

### Installation Steps

1. **Place the script**:  
   Copy `install.php` into your project’s `/public` directory.

2. **Open in browser**:  
   Navigate to the script in your browser, e.g.:
   ```
   http://localhost/your-project/public/install.php
   ```
3. **Follow the prompts:**

   The installer will guide you through configuration steps — including environment setup, database connection, and service initialization.

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

- PHP 8.3 or higher
- Composer
- A configured web server (Apache, Nginx, Caddy, etc.)
- Database connection (MySQL, PostgreSQL, or SQLite)



## Which Method Should You Choose?

| Environment | Recommended Method | Notes |
|--------------|--------------------|-------|
| Local development | Docker | Quickest and most reliable for development |
| Production server | Installer Script | Handles environment and dependencies automatically |
| Custom integration | Composer | Ideal for developers integrating SyncEngine into an existing project |

---

## Next Steps

Once SyncEngine is installed, continue with:

- [Setup Guide](setup.md)
- [Dashboard Overview](dashboard.md)
- [Tasks Reference](tasks.md)

---

> **Tip:** If you’re unsure which installation method to use, start with **Docker**.  
> It’s the easiest and fastest way to get SyncEngine up and running locally.
