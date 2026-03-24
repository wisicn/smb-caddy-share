# SMB + Caddy File Server

A simple Docker-based file sharing solution that provides both **SMB (Samba)** and **Web (HTTP)** access to your files.

## Features

- 📁 **Dual Access**: Share files via SMB mount and/or web browser
- 🐳 **One Command Setup**: Single `docker-compose.yml` for all services
- ⚙️ **Easy Configuration**: Customize via environment variables
- 🔄 **Persistent Storage**: Data persists across container restarts

## Quick Start

1. **Clone or download this project**

2. **Configure environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your preferred settings
   ```

3. **Start the services**
   ```bash
   docker compose up -d
   ```

4. **Access your files**
   - **SMB**: `smb://<server-ip>:2445`
   - **Web**: `http://<server-ip>:18808`

## Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `SHARED_DIR` | `./shared` | Host directory to share |
| `SMB_USER` | `samba` | SMB username |
| `SMB_PASS` | `secret` | SMB password |
| `PUID` | `1000` | User ID for file permissions (shared by both services) |
| `PGID` | `1000` | Group ID for file permissions (shared by both services) |
| `SMB_PORT` | `2445` | SMB port on host |
| `WEB_PORT` | `18808` | Web server port on host |

## Usage Examples

### Mount SMB Share

**macOS:**
```bash
# Via Finder: Cmd+K → smb://samba:secret@<server-ip>:2445/SharedFiles
# Or via terminal:
open smb://samba:secret@<server-ip>:2445/SharedFiles
```

**Linux:**
```bash
sudo mount -t cifs //<server-ip>:2445/SharedFiles /mnt/share \
  -o username=samba,password=secret,port=2445
```

**Windows:**
```
\\<server-ip>:2445\SharedFiles
```

### Access via Web Browser

Navigate to `http://<server-ip>:18808` to browse and download files through the web interface.

## Project Structure

```
.
├── docker-compose.yml    # Main compose file
├── Caddyfile             # Caddy server configuration
├── .env.example          # Environment variables template
├── .env                  # Your local configuration (create from .env.example)
├── .gitignore
└── README.md
```

## Stopping the Services

```bash
docker compose down
```

To also remove volumes:
```bash
docker compose down -v
```

## Security Notes

- Change the default `SMB_PASS` to a strong password
- Consider adding authentication to the web server if exposing publicly
- Both services share the same directory, so changes via SMB are immediately visible via web

## License

MIT
