# SFTP Webservice

## Overview

The **SFTP** webservice connects to SSH SFTP servers for secure file transfer. It supports key-based or username/password authentication and provides file and directory operations.

## When to Use

Use the SFTP webservice when you need to:

- Exchange files securely over SSH
- Meet compliance requirements for encrypted transport

## Settings

### Authentication

**Required** | **Type**: Mixed

- **Host**: SFTP host.
- **Port**: Default `22`.
- **Authentication type**: `private_key` or `username_password`.
- For `private_key`:
  - **Private key**: PEM content.
  - **Private key password**: Optional passphrase.
- For `username_password`:
  - **Username / Password**: Credentials.

## How It Works

1. **Client**: Establishes a secure SSH connection and logs in via the selected method. If login fails, the flow distinguishes between unreachable host and invalid credentials.
2. **Operations**: Same capabilities as FTP: get/put/delete/list/mkdir/rmdir/rename. Listing can filter by type; rename can override destination and includes clear error feedback on failure.

## Use Cases

### Secure Partner Exchange

**Configuration:**
- **Host**: `sftp.partner.com`
- **Auth method**: `private_key`

Result: Transfers CSV files securely.

## Best Practices

**Key Auth**: Prefer private keys; store secrets securely.

**Streaming**: Use temp resources for large uploads/downloads.

**Errors**: Capture and review error messages, especially for rename operations.

## Notes

- Provides detailed feedback for rename failures
