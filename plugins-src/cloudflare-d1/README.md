# Tabularis — Cloudflare D1 Plugin

Manage your [Cloudflare D1](https://developers.cloudflare.com/d1/) serverless SQLite databases from within Tabularis.

---

## Prerequisites

- A Cloudflare account with at least one D1 database
- A Cloudflare API Token with **D1 Edit** permissions (see below)

---

## Getting your credentials

### Account ID

1. Log in to the [Cloudflare dashboard](https://dash.cloudflare.com)
2. Select any domain (or go to the main account page)
3. In the right sidebar, copy the **Account ID** (a 32-character hex string)

### API Token

1. Go to **My Profile → API Tokens → Create Token**
2. Use the **"Edit Cloudflare Workers"** template, or create a custom token with:
   - **D1** → Edit
3. Click **Continue to summary → Create Token**
4. Copy the token — it is only shown once

---

## Connection setup in Tabularis

| Field        | Value                                                     |
|-------------|-----------------------------------------------------------|
| **Host**    | *(leave blank — the plugin connects to `api.cloudflare.com`)* |
| **Port**    | *(leave blank)*                                           |
| **Username**| Your Cloudflare **Account ID**                            |
| **Password**| Your Cloudflare **API Token**                             |
| **Database**| The **name** of your D1 database (e.g. `my-production-db`) |

> **Tip:** If you have multiple D1 databases, create a separate Tabularis connection for each one. The Account ID and API Token can be the same across all of them.

---

## Features

| Feature              | Supported |
|----------------------|-----------|
| Browse tables        | Yes       |
| Run SQL queries      | Yes       |
| Paginated results    | Yes       |
| Insert / Update / Delete rows | Yes |
| Create / drop tables | Yes       |
| Create / drop indexes | Yes      |
| Views (create / alter / drop) | Yes |
| Foreign key inspection | Yes    |
| ER diagram           | Yes       |
| Stored procedures    | No (D1/SQLite does not support them) |
| ALTER COLUMN         | No (SQLite limitation)              |

---

## Known limitations

- **No ALTER COLUMN** — SQLite does not support modifying an existing column definition. To change a column type or constraint you must recreate the table.
- **No DROP FOREIGN KEY** — Foreign keys must be defined at `CREATE TABLE` time. Removing one requires recreating the table.
- **No schemas** — D1 uses a flat single-schema model (same as SQLite).
- **Sequential operations** — The D1 HTTP REST API does not expose a true batch endpoint, so multi-statement operations (e.g. alter view) run as sequential calls rather than in a single atomic transaction.

---

## Installation location

The plugin must be placed in:

```
%APPDATA%\debba\tabularis\data\plugins\cloudflare-d1\
```

Which on a typical Windows install resolves to:

```
C:\Users\<your-username>\AppData\Roaming\debba\tabularis\data\plugins\cloudflare-d1\
```

The directory should contain:
- `cloudflare-d1-plugin.exe`
- `manifest.json`

Restart Tabularis after installing or updating the plugin.
