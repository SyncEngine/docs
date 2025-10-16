The **Connections** tab in SyncEngine is where all integrations begin.  
It serves as the central hub for managing external systems, defining credentials, and configuring secure data connections that power your automations.

With a unified interface, you can connect to APIs, file servers, databases, and the local filesystem — allowing SyncEngine to retrieve, transform, and send information across your entire ecosystem.

---

## Overview

Use the **Connections** tab to:

- Configure and manage integrations with external systems
- Authenticate and securely store credentials
- Test and validate connections in real time
- Reuse connections across multiple automations, flows, and routines
- Extend available connection types by installing modules

---

## Built-in Connection Types

SyncEngine ships with a rich set of built-in web service connectors to cover the most common integration needs. Each connector comes with its own configuration options and can be reused across automations.

| Connection Type | Description |
|------------------|-------------|
| **SFTP and FTP** | Secure file transfers using SFTP (SSH) or classic FTP. |
| **HTTP No Auth** | Make HTTP requests to endpoints without authentication. |
| **HTTP Basic Auth** | Access endpoints protected by basic username/password credentials. |
| **HTTP Bearer Token** | Authenticate with a static bearer token (e.g., API key, JWT). |
| **HTTP Auth Server** | Authenticate via an external authorization server that issues tokens dynamically. |
| **Local Filesystem** | Read or write files directly on the SyncEngine host. |
| **SOAP** | Integrate with SOAP-based services using WSDL definitions. |
| **SOAP Multistep Server** | Support complex SOAP workflows with multiple chained requests. |
| **SQL** | Connect to databases (MySQL, PostgreSQL, SQLite, MSSQL, etc.) for structured data exchange. |

> **Tip:** Install external modules to unlock even more connection types (for example, CRM or cloud service APIs).

---

## Credential Management

SyncEngine supports two secure storage mechanisms: **Vault Storage** and **Database Storage**. For credentials you should only use the Vault Storage, this is more secure and easy reuasble.
Some connections require temporary tokens, these can also be stored (and updated) in config of your connection.

### Vault Storage
Credentials are stored securely using Symfony Secrets Vault.  
Click the **+** icon beside a credential field to add a new secret. Provide a descriptive name and the value — this name will then appear as a reusable option in future configurations.
All credentials are encrypted locally in the Vault, ensuring they remain secure even if your database is ever compromised.
With Vault encryption, your credentials never leave your SyncEngine host unprotected even in the unlikely event of a database breach, your secrets stay safe and inaccessible.

## Creating a Connection

1. **Open** the **Connections** tab from the SyncEngine dashboard.
2. Click **Create new** and choose a connector type (FTP, HTTP, SQL, etc.).
3. **Configure** the connection fields such as host, credentials, port, and paths.
4. **Save** the connection — using Vault for secrets where possible.
5. **Test the connection** to verify connectivity and authentication press the "Connect" button. Once successful, it becomes available for any automation or flow.

---

## Security

- All credentials (in the vault) are encrypted.
- User access is managed through Symfony’s role-based permissions.
- System administrators can configure trusted IPs and environment-based access controls.
- SyncEngine never exposes secrets in logs or through the user interface.

---

## Troubleshooting

If your connection test fails:

1. Double-check hostnames, ports, and credentials.
2. Verify firewall or VPN settings that may restrict access.
3. Check whether the target service requires specific authentication headers.
4. Review the SyncEngine log for the exact error message.
5. For SQL connections, verify DSN formatting and ensure permissions for the user.

---

## Quick Reference: Configuration Fields

### SFTP and FTP
- **Protocol** (`sftp` or `ftp`)
- **Host**
- **Port** (22 for SFTP, 21 for FTP)
- **Passive mode** (on/off) (FTP only)
- **Username / Password** or **Private key (SFTP)**
- **Known hosts / Host key validation (SFTP)**
- **Timeout in seconds**
- **Root path**

### HTTP No Auth
- **Host**

### HTTP Basic Auth
- **Base URL**
- **Username / Password**

### HTTP Bearer Token
- **Base URL**
- **Bearer Token** (stored securely in Vault)

### HTTP Auth Server
- **Auth URL** (token endpoint)
- **Grant Type** (e.g., Client Credentials, Password)
- **Client ID / Secret** (stored in Vault)
- **Scope** (optional)
- **Token field mapping** (defines where to extract tokens)
- **API Base URL** (for subsequent calls)
- **Extra steps that your specific usecase could need**
- **Request timeout**
- **Request method**
- **Auto-refresh settings**

### Local Filesystem
- **Root directory** (absolute path)

### SOAP
- **WSDL URL**

### SOAP Multistep Server
- **Step definitions** (login → call → finalize, etc.)
- **Session state management** (cookies or headers)
- **Request/response chaining**
- **Error retry policy**
- **Response format**

### SQL
- **DSN / Driver** 
- **Port options** (if required)
- **Schema / Database name**
- **Username / Password**

---

## Best Practices

- Store all secrets in the Vault.
- Use least-privilege credentials for every connection.
- Group connections by environment (e.g., `crm.dev.api`, `crm.prod.api`).
- Reuse connections across multiple automations for consistency.
- Test connections regularly and update expiring tokens in advance.

---

By managing your connections effectively, SyncEngine becomes a unified bridge between systems securely transferring files, calling APIs, and querying databases with reliability and control.