# Setup Guide

After installing SyncEngine using Docker, the Installer Script, or Composer, you’ll be greeted with the **Setup screen** — the first step in configuring your SyncEngine instance.

![SyncEngine Setup](../img/setup/default-setup.png)

This screen allows you to configure the core system parameters, database connection, and administrator account.

---

## Initial Configuration

By default, SyncEngine uses an **SQLite database** stored in your local project directory.  
If you are just testing or running a small environment, you can use the default database path:

```bash
sqlite:///%kernel.project_dir%/var/data.db
```

You’ll find this pre-filled in the setup form.

If you prefer to connect SyncEngine to a different database (e.g., MySQL or PostgreSQL), simply replace this value with your own connection string, for example:

```bash
mysql://username:password@127.0.0.1:3306/syncengine
```

or

```bash
pgsql://username:password@127.0.0.1:5432/syncengine
```

> **Tip:** Only the database connection field is required for setup.  
> All other configuration options are optional and can be left empty unless you need custom settings.

---

## Configuration Fields Explained

Here’s what each option in the setup screen means:

| Field             | Description                                                                        |
|-------------------|------------------------------------------------------------------------------------|
| **Database URL**  | The connection string to your SyncEngine database. Default uses SQLite.            |
| **Secret**        | Optional, set a custom unique cryptographic keypreventing token or cookie conflicts across different installations                                   |
| **Mail Settings** | Optional, configure SMTP if you want SyncEngine to send notifications or reports. |

All these options can later be modified in your system settings or environment configuration file (`.env.local`) if needed.

---

## Creating the Admin Account

Once you press the **Install** button, SyncEngine will initialize the system and prompt you to create an **admin account**.

This account is for **SyncEngine administration only**, it’s not used for any connected services or external systems.

Enter the following:

- **Name** – Your admin display name
- **Email** – Used for login (no email verification required)
- **Password** – Choose a secure password

Click **Create Account** to finish.

---

## First Login

After the setup is complete, you can log in with your newly created admin account.

The SyncEngine dashboard will greet you with an overview of your system and the current status of your automations.

![SyncEngine Dashboard](../img/dashboard.png)

---

## Next Steps

Now that SyncEngine is configured, you can begin exploring its features:

- [Dashboard Overview](dashboard.md) – Learn about the interface and logs.
- [Connections](connections.md) – Start linking external systems.
- [Automations](automations.md) – Build your first automated workflow.
- [Tasks](tasks.md) – Understand the actions SyncEngine can perform.

---

> **Note:** The setup wizard only runs once. After installation, SyncEngine will automatically skip this step on subsequent launches.
