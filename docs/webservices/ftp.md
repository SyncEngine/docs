# FTP Webservice

## Overview

The **FTP** webservice connects to FTP/FTPS servers for uploading, downloading, and managing files and directories.

## When to Use

Use the FTP webservice when you need to:

- Perform legacy file transfers (FTP/FTPS)
- Push/pull batch files on schedules
- Manage remote directories and files

## Settings

### Authentication

**Required** | **Type**: Mixed

- **Host**: FTP host.
- **Port**: Default `21`.
- **Username / Password**: Credentials.
- **Passive mode**: Enable passive mode.
- **Connect using SSL**: Use FTPS (default `true`).
- **Process timeout**: Timeout in seconds (default `10`).

## How It Works

1. **Connect and Login**: Uses SSL or plain FTP; logs in with credentials.
2. **Passive Mode**: Sets passive mode according to configuration.
3. **Client Caching**: May cache the client per connection reference until timeout.
4. **Operations**:
   - **Get / Put**: Stream-safe file transfers (binary for put).
   - **Delete / Mkdir / Rmdir / Rename**: File and directory management.
   - **List**: Basic and detailed listing with optional type filters.
5. **Connect Result**: Returns `Result` with success or error info.

## Use Cases

### Import Partner CSV

**Configuration:**
- **Host**: `ftp.partner.com`
- **Passive**: Enabled

Result: Downloads CSV for processing.

### Nightly Report Export

**Configuration:**
- **Host**: `ftp.reports.local`
- **SSL**: Enabled

Result: Uploads PDFs to remote directory.

## Best Practices

**Passive Mode**: Typically required behind NAT/firewalls.

**FTPS**: Prefer SSL for secure transfers.

**Temp Resources**: Use temp files/streams for large uploads.

## Notes

- Detailed listing supports filtering by type (`file`/`dir`)
