<div align="center">

# 🚀 IRON-FILTER-BOT

### **A production-focused Telegram Auto-Filter • Search • Indexing • Streaming • Multi-Database Bot**

<p>
  <img src="https://img.shields.io/badge/Python-3.12%2B-3776AB?style=for-the-badge&logo=python&logoColor=white">
  <img src="https://img.shields.io/badge/wzgram-3.1.2-229ED9?style=for-the-badge&logo=telegram&logoColor=white">
  <img src="https://img.shields.io/badge/MongoDB-Multi--Database-47A248?style=for-the-badge&logo=mongodb&logoColor=white">
  <img src="https://img.shields.io/badge/aiohttp-Async-2C5BB4?style=for-the-badge&logo=aiohttp&logoColor=white">
  <img src="https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker&logoColor=white">
</p>

<p>
  <img src="https://img.shields.io/badge/Auto%20Filter-YES-success?style=flat-square">
  <img src="https://img.shields.io/badge/Streaming-YES-success?style=flat-square">
  <img src="https://img.shields.io/badge/Multi--DB-YES-success?style=flat-square">
  <img src="https://img.shields.io/badge/Web%20Admin%20Panel-YES-success?style=flat-square">
  <img src="https://img.shields.io/badge/MongoDB%20Manager-YES-success?style=flat-square">
  <img src="https://img.shields.io/badge/Live%20Config%20Reload-YES-success?style=flat-square">
</p>

<p>
  <b>🔥 SHOW-OFF BUILD • Developer Friendly • Admin Friendly • Deployment Friendly</b>
</p>

</div>

---

## 🧭 Navigation

<details open>
<summary><b>📚 Open the README sections</b></summary>

- [✨ What is IRON-FILTER-BOT?](#-what-is-iron-filter-bot)
- [🔥 SHOW-OFF — What makes this project different](#-show-off--what-makes-this-project-different)
- [🎯 Feature Matrix](#-feature-matrix)
- [🤖 Auto-Filter & Search](#-auto-filter--search-engine)
- [🗃️ Multi-Database Storage Engine](#️-multi-database-storage-engine)
- [⚡ Indexing Engine](#-indexing-engine)
- [🎬 TMDb / IMDb Metadata](#-tmdb--imdb-metadata)
- [🌐 Streaming & Download System](#-streaming--download-system)
- [🔗 File Links, Buttons & Shorteners](#-file-links-buttons--shorteners)
- [🕷️ Built-in Web Scrapers](#️-built-in-web-scrapers)
- [👤 User Experience](#-user-experience)
- [🔐 Access Control & Force Subscribe](#-access-control--force-subscribe)
- [📢 Broadcast & Administration](#-broadcast--administration)
- [🛡️ Standalone Web Admin Panel](#️-standalone-web-admin-panel)
- [🍃 MongoDB Manager](#-mongodb-manager)
- [♻️ Live Configuration Reload](#️-live-configuration-reload)
- [📊 Monitoring, Logs & Supervisor](#-monitoring-logs--process-supervisor)
- [🧰 Configuration](#-configuration)
- [⌨️ Command Reference](#️-command-reference)
- [🏗️ Project Architecture](#️-project-architecture)
- [🐳 Deployment](#-deployment)
- [⚙️ Performance & Reliability](#️-performance--reliability)
- [🔒 Security Notes](#-security-notes)
- [🧩 Development](#-development)
- [⚠️ Important Operational Notes](#️-important-operational-notes)

</details>

---

## ✨ What is IRON-FILTER-BOT?

**IRON-FILTER-BOT** is a Telegram automation project built around a searchable media/file index.

It combines the familiar functionality expected from an **Auto-Filter Bot** with a much larger administration and infrastructure layer:

> **Telegram Auto Filter + File Indexer + Multi-Database Storage + Streaming Server + Web Search + Web Admin Panel + MongoDB Browser + Process Supervisor**

The bot can index files from a Telegram database channel, extract searchable metadata, store records across one or many MongoDB file databases, search them from Telegram or the web, deliver selected results, generate protected links, stream files over HTTP, and manage the running system from a browser.

### 🧠 The main idea

```text
Telegram Files
      │
      ▼
┌──────────────────────┐
│   INDEXING ENGINE    │
│ metadata extraction  │
│ deduplication        │
│ TMDb/IMDb enrichment │
└──────────┬───────────┘
           │
           ▼
┌───────────────────────────────┐
│     MULTI-MONGODB STORAGE     │
│ DB #1 → DB #2 → DB #3 → ...   │
│ automatic storage monitoring  │
└──────────────┬────────────────┘
               │
       ┌───────┴────────┐
       ▼                ▼
 Telegram Search     Web Search
       │                │
       ▼                ▼
 Send / Select       Browse Results
 Stream / Download   HTTP Streaming
```

---

# 🔥 SHOW-OFF — What makes this project different

> This section intentionally separates features that are common in Auto-Filter bots from the parts that are more infrastructure-oriented or unusual.

## 🏆 The strongest SHOW-OFF features

### 1. 🗃️ Automatic Multi-Database File Storage

Instead of assuming that every indexed file must live in one MongoDB database, the project supports **multiple file-database URLs**.

The cluster manager:

- monitors database storage usage
- checks configured storage limits
- maintains database status
- selects an available database
- automatically switches to another database when the configured threshold is reached
- tracks indexing/storage statistics
- allows `/dbstatus` monitoring
- supports `FILES_DATABASE_URL`
- supports `DB_STORAGE_LIMIT_MB`
- supports `SWITCH_BUFFER_MB`
- supports `STORAGE_CHECK_INTERVAL`

**Why this stands out:** the database layer is designed around **storage expansion**, rather than treating MongoDB as a single permanent bucket.

---

### 2. 🛡️ A Standalone Web Admin Panel That Does NOT Depend on the Bot Process

The `webpanel/` application is deliberately separate from the Telegram bot.

That means the panel can remain available when the bot:

- crashes
- fails configuration validation
- has Telegram credential problems
- has a dependency/import failure
- exits unexpectedly

The panel can then show the failure and provide controls to restart or repair the bot.

**Architecture:**

```text
                ┌─────────────────────┐
                │  🌐 WEB ADMIN PANEL │
                │      :5000          │
                └─────────┬───────────┘
                          │
              start/stop/restart
                          │
                          ▼
                ┌─────────────────────┐
                │   🤖 BOT PROCESS    │
                │      :PORT          │
                └─────────────────────┘
```

This is one of the project's main infrastructure-level differentiators.

---

### 3. ♻️ Live Configuration Reload

The admin panel can save configuration to the environment/database and attempt to apply compatible changes to a **running bot without a restart**.

Reload paths include:

- same host → process signal
- separate host/container → authenticated HTTP reload endpoint
- `INTERNAL_RELOAD_TOKEN` protects the cross-host HTTP path

The bot updates its configuration dictionary **in place** for settings that are safe to change while running.

Some startup-bound values still require a restart, which is intentionally reported by the panel.

---

### 4. 🍃 Built-in MongoDB Manager

This is much more than a simple "database status" page.

The panel contains a browser-style MongoDB management interface capable of:

- connecting to MongoDB URIs
- listing databases
- listing collections
- showing collection/document statistics
- finding documents
- filtering
- sorting
- projection
- pagination
- inserting documents
- replacing documents
- editing documents
- cloning documents
- deleting documents
- bulk update
- bulk delete
- aggregation pipelines
- aggregation preview
- schema sampling
- index inspection
- index creation
- index deletion
- explain plans
- JSON import
- Extended JSON export
- `mongosh` output
- PyMongo script output
- read-only mode

And importantly, **read-only enforcement happens server-side**, not merely by hiding browser buttons.

---

### 5. ☑️ Multi-Page File Selection

A conventional filter result usually gives the user a list and a "send all" style action.

This project also supports:

- selecting individual files
- selecting files across multiple result pages
- deselecting
- "Send Selected"
- configurable `MAX_SELECT_FILES`
- selection state protection
- protection against stale/old result messages

This gives users granular control over large search results without forcing the bot to send every match.

---

### 6. 🎬 Metadata-Aware Search

Indexed files can carry extracted metadata such as:

- file name
- file size
- language
- quality
- season
- episode
- year
- file type
- MIME type
- caption
- creation/index time

The search system can filter by:

**Quality • Language • Season • Episode • Year**

And can optionally search captions in addition to filenames.

---

### 7. 🕷️ Built-in Multi-Source Scraper Layer

The project includes scraper integrations for configured sources including:

- SkyMoviesHD
- MoviesDrive
- 4KHDHub
- LuxMovies
- MoviesUp

The `/search` workflow can search configured scraper sources, paginate results, and generate source-specific deep-link flows.

This is separate from the normal MongoDB Auto-Filter search.

---

### 8. 🌐 Telegram + Web Search

The indexed database is not limited to Telegram UI.

The bot also exposes an HTTP search interface with:

- query search
- pagination
- quality filtering
- language filtering
- season filtering
- episode filtering
- year filtering
- file metadata display

The same system also provides HTTP file streaming/download functionality.

---

# 🎯 Feature Matrix

| Feature | Common in Auto-Filter bots | IRON-FILTER-BOT |
|---|:---:|:---:|
| Telegram filename search | ✅ | ✅ |
| File indexing | ✅ | ✅ |
| Quality filtering | ✅ | ✅ |
| Language filtering | ✅ | ✅ |
| Season/Episode filtering | ✅ | ✅ |
| Year filtering | ✅ | ✅ |
| Force Subscribe | ✅ | ✅ |
| Admin/Sudo system | ✅ | ✅ |
| Broadcast | ✅ | ✅ |
| User settings | ✅ | ✅ |
| Auto-delete messages | ✅ | ✅ |
| IMDb/TMDb metadata | ◐ | ✅ |
| Streaming links | ◐ | ✅ |
| HTTP web search | ◐ | ✅ |
| Multi-client streaming | ◐ | ✅ |
| Built-in external scrapers | ◐ | ✅ |
| Select files across pages | ◐ | ⭐ |
| Automatic multi-Mongo storage | ❌/rare | 🔥 |
| Storage-aware DB switching | ❌/rare | 🔥 |
| Standalone admin process | ❌/rare | 🔥 |
| Bot process supervisor | ❌/rare | 🔥 |
| Live config reload | ❌/rare | 🔥 |
| Browser MongoDB Manager | ❌ | 🔥 |
| Mongo aggregation builder | ❌ | 🔥 |
| Mongo schema/index tools | ❌ | 🔥 |
| Panel audit log | ❌ | 🔥 |
| Panel TOTP 2FA | ❌/rare | 🔥 |
| Panel IP allowlist | ❌/rare | 🔥 |
| Config backup/restore | ❌/rare | 🔥 |
| Same-host signal reload | ❌/rare | 🔥 |
| Cross-host authenticated reload | ❌/rare | 🔥 |
| Crash output shown in panel | ❌/rare | 🔥 |
| Per-process CPU/RAM monitoring | ❌/rare | 🔥 |
| Automatic bot crash restart | ◐ | 🔥 |
| Storage cluster statistics | ❌/rare | 🔥 |

> **Legend:** `✅` implemented • `◐` depends on bot/project • `🔥` infrastructure-level differentiator.

---

# 🤖 Auto-Filter & Search Engine

## 🔎 Core Search

Users can search the indexed database directly from Telegram.

Supported search metadata includes:

- 🎞️ File name
- 💾 File size
- 🌍 Language
- 🎥 Quality
- 📺 Season
- 🎬 Episode
- 📅 Year
- 📄 File type
- 🧾 Caption
- MIME type

Search can optionally use captions as an additional searchable field.

### 🎛️ Filtering

The result system supports:

```text
Query
 ├── Language
 ├── Quality
 ├── Season
 ├── Episode
 └── Year
```

### 📄 Pagination

Web search currently renders results in pages of **20 results**.

Telegram filtering also supports result-page navigation.

### ✨ Spell / search assistance

The Auto-Filter module contains spelling-oriented handling for search terms and configurable spell/result messaging.

---

## ☑️ Select Mode

When enabled:

```text
Search
  ↓
Result Page 1
  ├─ ☑ File A
  ├─ ☐ File B
  └─ ☑ File C
        ↓
Next Page
  ├─ ☑ File D
  └─ ☐ File E
        ↓
📤 SEND SELECTED
```

Users can select files across result pages and receive only the chosen items.

The maximum number delivered in one selected-send operation is configurable with:

```env
MAX_SELECT_FILES=
```

---

## 📦 Send All

The project can expose a **Send All** action for search results.

This is configurable:

```env
SEND_ALL_BUTTON=
```

---

# 🗃️ Multi-Database Storage Engine

This is a core architectural feature of the project.

## Database roles

### Main database

`DATABASE_URL`

Used for application-level data such as:

- user data
- bot settings
- PM user records
- chat records
- tokens
- invite data
- force-subscribe data
- other application state

### File databases

`FILES_DATABASE_URL`

Used for indexed media/file documents.

Multiple MongoDB URLs can be configured.

Example concept:

```env
FILES_DATABASE_URL="mongodb+srv://DB1 ... mongodb+srv://DB2 ... mongodb+srv://DB3 ..."
```

The exact separator/format should follow the project's configuration comments.

---

## 🔄 Storage-aware database switching

The `TelegramClusterManager` monitors configured file databases.

Important controls:

| Variable | Purpose |
|---|---|
| `DB_STORAGE_LIMIT_MB` | Maximum storage target for a file database |
| `SWITCH_BUFFER_MB` | Safety buffer before switching |
| `STORAGE_CHECK_INTERVAL` | Storage recheck interval |
| `SAVE_FILE_CONCURRENCY` | Bounded concurrent save operations |

Conceptually:

```text
DB #1
  │
  ├── storage below threshold ──► keep indexing
  │
  └── threshold reached
              │
              ▼
          DB #2
              │
              ▼
          DB #3
              │
             ...
```

### 📊 Database status

The bot exposes database status information through:

```text
/dbstatus
```

The panel can also display database statistics.

---

# ⚡ Indexing Engine

The indexing system is designed for Telegram database channels.

## 📥 Indexing workflow

```text
Forward database-channel message
             ↓
        /index
             ↓
       Choose direction
        ↙          ↘
   Up → Down     Down → Up
             ↓
       Read messages
             ↓
    Extract file metadata
             ↓
       Optional TMDb data
             ↓
       Save to MongoDB
```

## ↕️ Index direction

The indexer supports:

- **Up to Down**
- **Down to Up**

This is useful when maintaining or rebuilding an existing database channel.

---

## ⏭️ Skip IDs

The project supports skipping selected Telegram message IDs during indexing.

Commands include:

```text
/setskip
```

The implementation maintains skip IDs per relevant source/channel and supports indexing with skipped files.

---

## ⛔ Cancel indexing

An active indexing operation can be stopped through its interactive control flow.

The bot prevents another indexing process from starting while an indexing job is already active.

---

## 🚀 Bounded concurrent indexing

Network-heavy file-saving work can be executed concurrently using:

```env
SAVE_FILE_CONCURRENCY=
```

The concurrency is bounded rather than unlimited, reducing the chance of turning a large indexing job into an uncontrolled request storm.

---

# 🎬 TMDb / IMDb Metadata

The metadata layer integrates with **TMDb** for title information.

Depending on configuration/result type, metadata can include:

- 🎞️ Title
- 🖼️ Poster
- ⭐ Rating-related information
- 📝 Plot/overview
- 👥 Cast/crew-related metadata
- 🔎 External IDs
- 🎬 Movie/TV details
- 🎟️ Certificate information
- Alternative titles

Configuration:

```env
TMDB_API=
IMDB_RESULT=
LONG_IMDB_DESCRIPTION=
IMDB_TEMPLATE_TXT=
```

## 🧩 Custom result template

`IMDB_TEMPLATE_TXT` lets the administrator customize the metadata caption format instead of being locked to one fixed result message.

---

# 🌐 Streaming & Download System

The project contains an HTTP server for file access.

## 🎥 Stream mode

```env
STREAM_MODE=
BOT_BASE_URL=
PORT=
```

When enabled and correctly exposed, the bot can generate HTTP streaming/download links for Telegram files.

---

## 🔐 Secure file URLs

The project contains hash-based file-link validation.

Relevant configuration:

```env
FILE_SECURE_MODE=
```

The streaming layer validates file identifiers/hashes and handles invalid/missing file cases.

---

## 📦 Direct download

The HTTP layer supports direct file downloading in addition to streaming.

The implementation includes a custom byte-streaming component for serving Telegram media without requiring the complete file to be permanently stored on the web server.

---

## ⚡ Multi-client support

The project supports optional multiple Telegram clients for parallelized media delivery:

```env
MULTI_CLIENT=
```

This is useful when scaling streaming/download workloads.

---

## 🧹 Download cleanup

The project includes download cleanup helpers and configurable file/message deletion behavior.

---

# 🔗 File Links, Buttons & Shorteners

The bot contains several mechanisms for extending result/file messages.

### Available workflows

- Generate file links
- Add file buttons
- Add download-link buttons
- Add extra file buttons
- Add extra request-source buttons
- Generate streaming/download callbacks
- Optional URL shortening
- Optional external hosting integrations

Commands include:

```text
/genlink
/addfbtn
/adl
/stream
```

Configured external services include:

```env
SHORT_URL_API=
EARNVIDS_API=
STREAMP2P_API=
```

---

# 🕷️ Built-in Web Scrapers

The project contains a dedicated `bot/web_scrapper/` layer.

Configured scraper modules include:

| Source module | Purpose |
|---|---|
| `skymoviehd.py` | SkyMoviesHD source |
| `moviesdrive.py` | MoviesDrive source |
| `fourkhdhub.py` | 4KHDHub source |
| `luxmovies.py` | LuxMovies source |
| `moviesup.py` | MoviesUp source |
| `scrape_handler.py` | Search/pagination handling |

Configuration:

```env
SKYMVHD_URL=
MVDRIVE_URL=
FOURKHDHUB_URL=
LUXMOVIES_URL=
MOVIESUP_URL=
```

The scraper system is separate from the MongoDB Auto-Filter index, allowing source searching without requiring those results to already exist in the bot's indexed database.

---

# 👤 User Experience

## ⚙️ Per-user settings

Users can open:

```text
/usersettings
/us
```

The project stores user-specific settings and provides an interactive settings interface.

---

## 🧹 Automatic message cleanup

Configurable cleanup features include:

- auto-delete incoming user messages
- auto-delete filter result messages
- configurable result-message timeout
- auto-delete delivered files
- configurable delivered-file timeout

Relevant variables:

```env
AUTODELICMINGUSERMSG=
AUTO_DEL_FILTER_RESULT_MSG=
AUTO_DEL_FILTER_RESULT_MSG_TIMEOUT=
AUTO_FILE_DELETE_MODE=
AUTO_FILE_DELETE_MODE_TIMEOUT=
```

> ⚠️ `AUTO_FILE_DELETE_MODE` has an interaction with secure file mode; follow the configuration comments in `sample_config.env`.

---

## 😀 Reactions

Optional reaction behavior is configurable through:

```env
EMOJI_REACT=
EMOJI_BIG=
EMOJIS_LIST=
```

---

## 📝 Fully customizable bot text

The configuration supports custom text for areas including:

- Start message
- Result text
- Help
- About
- Admin help
- Source text
- Disclaimer
- No-result messages
- Movie-not-found messages
- File-not-found messages
- Alerts
- Spell/search image
- Button links

This allows the same codebase to be branded for different deployments.

---

# 🔐 Access Control & Force Subscribe

## 👑 Authorization

The bot supports:

- authorized users
- sudo users
- authorized chats
- owner-level operations
- admin-only commands

Commands include:

```text
/authorize
/unauthorize
/addsudo
/rmsudo
```

---

## 📢 Force Subscribe

The bot supports configurable force-subscription channels:

```env
FSUB_IDS=
REQ_JOIN_FSUB=
USENEWINVTLINKS=
```

The implementation includes support for request-to-join style force subscription.

---

## 🎫 User access tokens

The project includes temporary user token handling.

Configured with:

```env
TOKEN_TIMEOUT=
```

Token data can be stored through the database layer and validated when users redeem access.

---

## 🧩 Command suffix

`CMD_SUFFIX` allows command names to be customized/suffixed.

This can be useful when operating multiple bots or command namespaces in the same environment.

---

# 📢 Broadcast & Administration

## 📣 Broadcast

The bot can broadcast messages to stored PM users.

```text
/broadcast
```

The database layer maintains PM-user information used by the broadcast workflow.

---

## 🗑️ Database file deletion

Administrators can remove indexed file records.

```text
/deletefile
/df

/deletefiles
/dfs
```

Bulk deletion supports operations based on criteria such as date/keyword according to the implemented workflow.

---

## 📊 Bot/system statistics

The bot exposes machine/bot information including:

- bot uptime
- system uptime
- CPU usage
- RAM usage
- disk usage
- free disk space
- total disk space

Commands:

```text
/stats
/bstats
/ping
```

---

## 📜 Logs

Administrators can retrieve the bot log:

```text
/log
```

The web server also exposes log-related pages/routes.

---

## 🔄 Restart

The bot supports:

```text
/restart
/r
```

The restart workflow can update the project and restart the Python bot process according to the configured update mechanism.

---

# 🛡️ Standalone Web Admin Panel

The `webpanel/` directory provides a dedicated browser-based control plane.

## 🎛️ Panel capabilities

### 📊 Overview

The panel can provide:

- bot status
- start/stop/restart controls
- auto-restart control
- bot uptime
- panel uptime
- host CPU
- RAM
- disk
- bot process CPU
- bot process RAM
- thread count
- crash/exit information
- recent bot output

---

## ♻️ Crash supervision

The panel includes a `BotSupervisor`.

It can:

- launch the bot
- stop the bot
- restart the bot
- monitor the process
- detect an externally running bot
- collect bot output
- keep bot logs
- restart after crashes when enabled
- stop retrying after repeated failures

The configured automatic restart policy gives up after **5 consecutive failures**, preventing an endless crash loop.

---

## ⚙️ Settings UI

The panel can expose the project's configuration variables with:

- type-aware controls
- descriptions
- variable search/filter
- source indication
- secret masking
- dirty-change tracking
- save/discard workflow
- dual write to `config.env` and MongoDB where supported
- automatic configuration backups
- live-apply handling
- restart-required reporting

---

## 💾 Configuration backups

Before configuration saves, the panel can create timestamped `config.env` backups.

The panel retains recent backups and provides restore functionality.

This protects against accidental configuration changes.

---

# 🔐 Panel Security

The panel contains several security mechanisms.

## 🔑 Password security

Password handling uses PBKDF2-based password hashing/verification support.

---

## 🔢 TOTP 2FA

The panel supports RFC 6238-style TOTP authentication.

Configuration:

```env
ADMIN_TOTP_SECRET=
```

The panel can generate/setup TOTP provisioning information and verify the second factor during login.

---

## 🧱 CSRF protection

State-changing panel requests require a CSRF token.

---

## 🚫 Login rate limiting / lockout

Failed authentication attempts are rate-limited and can trigger escalating lockout behavior.

This is intended to reduce brute-force attempts against the panel.

---

## 🌍 IP allowlist

Optional IP/CIDR restrictions are supported:

```env
PANEL_IP_ALLOWLIST=
```

When configured, requests outside the allowlist are rejected.

---

## 🧾 Audit log

Security-sensitive and state-changing panel actions can be written to the audit log, including actions such as:

- login success/failure
- logout
- bot controls
- settings changes
- live reload
- password changes
- TOTP changes
- session revocation
- MongoDB writes
- database connections

---

## 🔓 Session management

The panel includes session creation, expiration and revocation functionality.

Administrators can revoke active sessions, forcing users back through authentication.

---

# 🍃 MongoDB Manager

## 🧭 Compass-style database browsing

The MongoDB Manager can connect to:

```text
mongodb://...
mongodb+srv://...
```

It can also discover configured database sources such as:

- `DATABASE_URL`
- `FILES_DATABASE_URL`

---

## 🗂️ Database & collection operations

The manager can:

- list databases
- list collections
- show document counts
- show storage statistics
- create collections/databases where permitted
- drop collections/databases with confirmation
- inspect indexes

---

## 🔎 Document explorer

Supported operations include:

- filter
- projection
- sorting
- pagination
- document inspection
- document insertion
- document replacement
- document deletion
- bulk updates
- bulk deletes
- cloning

Pagination is bounded by the server-side maximum.

---

## 🧬 BSON-aware editing

The manager uses canonical Extended JSON handling and supports BSON-oriented types such as:

- String
- Int32
- Int64
- Double
- Decimal128
- Boolean
- Date
- ObjectId
- Object
- Array
- Null
- RegExp
- Timestamp
- Binary
- Code
- MinKey
- MaxKey

This is particularly useful when working with real MongoDB data rather than plain JSON.

---

## 🧪 Aggregation builder

The manager supports MongoDB aggregation pipelines with:

- stage builder
- stage preview
- text mode
- sample mode
- live output
- controlled execution

---

## 📐 Schema & index tools

The manager can inspect:

- field types
- common sampled values
- indexes
- index size
- index usage information

It can create/drop supported indexes with options such as:

- unique
- sparse
- hidden
- TTL
- partial
- text

---

## 🧠 Explain plans

MongoDB query explain functionality is available for investigating query behavior and database performance.

---

## 📤 Import / Export

Supported workflows include:

- JSON array import
- MongoDB one-document-per-line import
- Extended JSON export
- `mongosh` syntax output
- PyMongo script output

---

## 🚨 Server-side read-only mode

The MongoDB manager includes a read-only switch.

Importantly, write blocking is enforced in the **server-side manager**, not only by the browser UI.

---

# ♻️ Live Configuration Reload

One of the more unusual infrastructure features is the panel-to-bot reload path.

```text
                 SAVE SETTING
                      │
                      ▼
             ┌─────────────────┐
             │   WEB PANEL     │
             └────────┬────────┘
                      │
              config.env + DB
                      │
             ┌────────┴────────┐
             │                 │
        Same host         Different host
             │                 │
          SIGHUP       HTTP reload endpoint
             │          + reload token
             └────────┬────────┘
                      ▼
                RUNNING BOT
                      │
               reload_config_dict()
                      │
             ┌────────┴─────────┐
             │                  │
         Hot-reloadable     Restart required
```

The panel reports which changes were:

- applied live
- unable to reach the bot
- restart-required

### 🔒 Cross-host protection

For separate hosts/containers:

```env
INTERNAL_RELOAD_TOKEN=
```

must be configured consistently.

The bot exposes:

```text
POST /internal/reload-config
```

and validates:

```text
X-Reload-Token
```

---

# 📊 Monitoring, Logs & Process Supervisor

## 🖥️ Host monitoring

The project uses `psutil` for system/process metrics.

Tracked information includes:

- CPU
- CPU cores
- load-related system information
- RAM
- disk
- bot process memory
- bot process CPU
- thread count
- uptime

---

## 🧾 Bot output handling

The supervisor captures bot output into a file instead of relying on an undrained subprocess pipe.

This avoids the classic problem where a child process can eventually block because its output pipe fills.

---

## 🔒 Single-instance protection

The bot uses an instance lock to reduce accidental duplicate bot processes.

This matters because Telegram client session files are not designed for multiple independent writers.

---

# 🧰 Configuration

A complete template is provided:

```text
sample_config.env
```

The project contains configuration for the following areas.

<details>
<summary><b>🤖 Core Telegram configuration</b></summary>

```env
BOT_TOKEN=
TELEGRAM_API=
TELEGRAM_HASH=
OWNER_ID=
DATABASE_CHANNEL=
LOG_CHANNEL=
BOT_BASE_URL=
DATABASE_URL=
```

</details>

<details>
<summary><b>🗃️ Database / cluster configuration</b></summary>

```env
FILES_DATABASE_URL=
DB_STORAGE_LIMIT_MB=
SWITCH_BUFFER_MB=
STORAGE_CHECK_INTERVAL=
SAVE_FILE_CONCURRENCY=
```

</details>

<details>
<summary><b>🌐 Web server / streaming</b></summary>

```env
PORT=
STREAM_MODE=
STREAM_BIN_CHNL_ID=
FILE_BIN_CHANNEL=
FILE_SECURE_MODE=
MAXX_DL_LIMIT=
MAXX_UP_LIMIT=
KEEP_ALIVE=
PING_INTERVAL=
```

</details>

<details>
<summary><b>👮 Access control</b></summary>

```env
SUDO_USERS=
CMD_SUFFIX=
FSUB_IDS=
REQ_JOIN_FSUB=
USENEWINVTLINKS=
TOKEN_TIMEOUT=
SET_COMMANDS=
```

</details>

<details>
<summary><b>⚡ Performance</b></summary>

```env
BOT_WORKERS=
MAX_CONCURRENT_TRANSMISSIONS=
SLEEP_THRESHOLD=
MULTI_CLIENT=
MAX_LIST_ELM=
MEDIA_ANALYSIS_LIMIT_MB=
```

</details>

<details>
<summary><b>🔎 Search / filter / result controls</b></summary>

```env
USE_CAPTION_FILTER=
SEND_ALL_BUTTON=
FILTERING_DATA_BUTTON=
SELECT_BUTTON=
MAX_SELECT_FILES=
NO_RESULTS_MSG=
AUTO_DEL_FILTER_RESULT_MSG=
AUTO_DEL_FILTER_RESULT_MSG_TIMEOUT=
AUTODELICMINGUSERMSG=
AUTO_FILE_DELETE_MODE=
AUTO_FILE_DELETE_MODE_TIMEOUT=
CUSTOM_FILE_CAPTION=
```

</details>

<details>
<summary><b>🎬 TMDb / IMDb</b></summary>

```env
IMDB_RESULT=
LONG_IMDB_DESCRIPTION=
IMDB_TEMPLATE_TXT=
TMDB_API=
```

</details>

<details>
<summary><b>🎨 Messages / branding / buttons</b></summary>

The project provides configurable text and URL fields for the bot's:

- start screen
- result screen
- help
- about
- admin help
- source
- disclaimer
- alerts
- no-result responses
- owner/support/update/repository buttons
- movie channel links
- bot channel links

</details>

<details>
<summary><b>🔗 Shorteners / hosting / scrapers</b></summary>

```env
SHORT_URL_API=
EARNVIDS_API=
STREAMP2P_API=

SKYMVHD_URL=
MVDRIVE_URL=
FOURKHDHUB_URL=
LUXMOVIES_URL=
MOVIESUP_URL=
```

</details>

<details>
<summary><b>🌐 Web Admin Panel</b></summary>

```env
ADMIN_USERNAME=
ADMIN_PASSWORD=
ADMIN_TOTP_SECRET=
PANEL_PORT=
PANEL_HTTPS=
PANEL_IP_ALLOWLIST=
PANEL_AUTOSTART_BOT=
PANEL_AUTORESTART_BOT=
INTERNAL_RELOAD_TOKEN=
```

</details>

<details>
<summary><b>🔄 Update system</b></summary>

```env
UPSTREAM_REPO=
UPSTREAM_BRANCH=
USER_SESSION_STRING=
```

> ⚠️ `UPSTREAM_REPO` can contain credentials/tokens depending on deployment. Never publish secrets in a public repository.

</details>

---

# ⌨️ Command Reference

> `CMD_SUFFIX` is applied to many commands. The examples below show the default command names.

## 👤 User commands

| Command | Purpose |
|---|---|
| `/start` | Start bot / process start payloads |
| `/help` | Help |
| `/id` | Get user/chat ID |
| `/stickerid` | Get sticker ID |
| `/usersettings` | Open user settings |
| `/us` | Alias for user settings |
| `/media_info` | Extract media information |
| `/minfo` | Alias for media information |
| `/minfo_json` | Get file information in JSON form |
| `/stream` | Generate streaming/download workflow |
| `/search` | Search configured scraper sources |
| `/ping` | Check response timing |

## 👑 Admin / Sudo commands

| Command | Purpose |
|---|---|
| `/index` | Start database-channel indexing |
| `/setskip` | Configure skipped IDs |
| `/dbstatus` | Show database/storage status |
| `/deletefile` | Delete one indexed file |
| `/df` | Alias for delete file |
| `/deletefiles` | Bulk database-file deletion |
| `/dfs` | Alias for bulk deletion |
| `/authorize` | Authorize user/chat |
| `/unauthorize` | Remove authorization |
| `/addsudo` | Add sudo user |
| `/rmsudo` | Remove sudo user |
| `/botsettings` | Open bot settings |
| `/bs` | Alias for bot settings |
| `/broadcast` | Broadcast to PM users |
| `/stats` | Machine/bot statistics |
| `/bstats` | Bot statistics alias |
| `/ping` | Ping |
| `/log` | Retrieve logs |
| `/restart` | Restart bot |
| `/r` | Restart alias |
| `/genlink` | Generate file link |
| `/addfbtn` | Add file button |
| `/adl` | Add download link |
| `/getfsubdata` | Inspect force-subscription data |
| `/delpmuser` | Delete stored PM user |
| `/delfsubuser` | Delete force-sub user |
| `/chats_list` | List chats |
| `/checkrights` | Check bot rights |

---

# 🏗️ Project Architecture

```text
IRON-FILTER-BOT/
│
├── bot/
│   ├── __init__.py
│   ├── __main__.py
│   ├── config_meta.py
│   │
│   ├── database/
│   │   ├── db_handler.py
│   │   ├── db_file_handler.py
│   │   ├── db_utils.py
│   │   └── telegram_cluster_manager.py
│   │
│   ├── plugins/
│   │   ├── autofilter.py
│   │   ├── index.py
│   │   ├── route.py
│   │   ├── commands.py
│   │   ├── bot_settings.py
│   │   ├── user_settings.py
│   │   ├── authorize.py
│   │   ├── broadcast.py
│   │   ├── delete_dbfiles.py
│   │   ├── stream_download.py
│   │   ├── database_channel.py
│   │   └── ...
│   │
│   ├── web_scrapper/
│   │   ├── skymoviehd.py
│   │   ├── moviesdrive.py
│   │   ├── fourkhdhub.py
│   │   ├── luxmovies.py
│   │   ├── moviesup.py
│   │   └── scrape_handler.py
│   │
│   ├── helper/
│   │   ├── hoster/
│   │   ├── content_utils/
│   │   ├── extra/
│   │   └── telegram_helper/
│   │
│   ├── iron/
│   │   └── util/
│   │
│   ├── templates/
│   └── assets/
│
├── webpanel/
│   ├── app.py
│   ├── security.py
│   ├── supervisor.py
│   ├── config_store.py
│   ├── mongo_manager.py
│   ├── static/
│   └── templates/
│
├── sample_config.env
├── docker-compose.yml
├── Dockerfile
├── start.sh
├── update.py
└── requirements.txt
```

---

# 🐳 Deployment

## Option 1 — Docker Compose

The repository includes:

```text
docker-compose.yml
```

Typical workflow:

```bash
git clone <YOUR-REPOSITORY>
cd IRON-FILTER-BOT

cp sample_config.env config.env
nano config.env

docker compose up -d panel
```

The exact service names/commands should be checked against the included `docker-compose.yml`.

---

## Option 2 — Docker

Build:

```bash
docker build -t iron-filter-bot .
```

Run the application according to the ports and command defined by your deployment configuration.

If streaming is enabled, make sure the configured `PORT` is published externally and matches the public `BOT_BASE_URL`.

---

## Option 3 — VPS / Bare Metal

Install dependencies:

```bash
pip3 install -r requirements.txt
```

Start the bot:

```bash
python3 -m bot
```

Start the admin panel separately:

```bash
python3 -m webpanel
```

---

## 🔌 Important ports

Typical architecture:

| Service | Port |
|---|---:|
| Telegram bot HTTP/streaming server | `PORT` |
| Web Admin Panel | `PANEL_PORT` |
| Default panel configuration | `5000` |

Do not configure the panel and bot to bind to the same port.

---

# ⚙️ Performance & Reliability

## 🚀 Async architecture

The project uses asynchronous components including:

- `asyncio`
- `aiohttp`
- async MongoDB access
- asynchronous Telegram operations
- concurrent bounded indexing tasks

---

## 📦 Bounded save concurrency

Indexing can overlap network-bound save work while respecting a configured concurrency limit:

```env
SAVE_FILE_CONCURRENCY=
```

---

## 🧵 Worker configuration

Telegram client workers can be tuned with:

```env
BOT_WORKERS=
```

Transmission concurrency can be controlled with:

```env
MAX_CONCURRENT_TRANSMISSIONS=
```

---

## ⏱️ FloodWait handling

The project includes configurable sleep-threshold behavior:

```env
SLEEP_THRESHOLD=
```

Short waits can be handled automatically rather than immediately failing the operation.

---

## 🧹 Resource cleanup

The project contains cleanup helpers for temporary downloads/tasks and supports controlled task/download/upload concurrency.

---

# 🔒 Security Notes

## Never commit secrets

Do **not** commit:

```text
config.env
BOT_TOKEN
TELEGRAM_HASH
DATABASE_URL
FILES_DATABASE_URL
TMDB_API
ADMIN_PASSWORD
ADMIN_TOTP_SECRET
INTERNAL_RELOAD_TOKEN
USER_SESSION_STRING
UPSTREAM_REPO credentials
```

Use the supplied `.gitignore` and keep deployment secrets outside the repository.

---

## ⚠️ User session strings

`USER_SESSION_STRING` can provide powerful access to a Telegram user account.

Treat it like a password or private credential.

---

## 🌐 Exposing the panel

If the panel is public:

- use a strong admin password
- enable TOTP
- consider `PANEL_IP_ALLOWLIST`
- use HTTPS/TLS
- avoid exposing MongoDB credentials
- rotate compromised credentials immediately

---

# 🧩 Development

## Requirements

The current repository declares dependencies including:

- `wzgram==3.1.2`
- `aiohttp`
- `motor`
- `pymongo`
- `python-dotenv`
- `apscheduler`
- `psutil`
- `jinja2`
- `beautifulsoup4`
- `cloudscraper`
- `ffmpeg-python`
- `telegraph`
- `httpx`
- `parse-torrent-title`
- and related runtime packages

Install with:

```bash
pip3 install -r requirements.txt
```

---

## 🧱 Main modules for developers

| Module | Responsibility |
|---|---|
| `bot/plugins/autofilter.py` | Telegram Auto-Filter/result selection |
| `bot/plugins/index.py` | Indexing workflow |
| `bot/database/db_file_handler.py` | File DB models/storage |
| `bot/database/db_utils.py` | Search/query helpers |
| `bot/database/telegram_cluster_manager.py` | Multi-DB storage switching |
| `bot/plugins/route.py` | HTTP search/stream/reload routes |
| `bot/helper/content_utils/tmdb.py` | TMDb metadata |
| `bot/web_scrapper/` | External search/scraper sources |
| `webpanel/app.py` | Admin panel routes/UI API |
| `webpanel/security.py` | Auth/security/session layer |
| `webpanel/supervisor.py` | Bot process lifecycle |
| `webpanel/config_store.py` | Config persistence/backup |
| `webpanel/mongo_manager.py` | MongoDB browser/API |

---

# 🧠 Why this project is more than a normal Auto-Filter Bot

A normal Auto-Filter Bot is generally centered around:

```text
Search → Find file → Send file
```

IRON-FILTER-BOT expands that into:

```text
                    ┌───────────────┐
                    │ Telegram Bot  │
                    └───────┬───────┘
                            │
              ┌─────────────┼─────────────┐
              ▼             ▼             ▼
          Auto Filter    Indexing      Scrapers
              │             │             │
              └──────┬──────┴──────┬──────┘
                     ▼             ▼
                 Metadata      Multi-DB
                     │          Storage
                     └──────┬───────┘
                            ▼
                    Streaming / HTTP
                            │
              ┌─────────────┴─────────────┐
              ▼                           ▼
        Telegram Users              Web Search
              │                           │
              └─────────────┬─────────────┘
                            ▼
                    Web Admin Panel
                            │
        ┌───────────────┬───┴───────────────┐
        ▼               ▼                   ▼
   Bot Supervisor   Config Manager     MongoDB Manager
        │               │                   │
        ▼               ▼                   ▼
   Start/Stop       Live Reload       Query/Edit/Aggregate
```

That is the central **SHOW-OFF** point of the project:

> **It is not only a Telegram filter bot; it is a Telegram media-indexing and delivery system with its own administration and database-management layer.**

---

# ⭐ Feature Highlights for Developers

If you are evaluating the repository as a developer, these are the areas worth inspecting first:

### 🔥 Infrastructure

- Multi-Mongo file storage
- Automatic storage-aware database switching
- Standalone panel process
- Bot process supervision
- Automatic crash restart
- Same-host and cross-host live reload
- Config backup/restore

### 🔥 Database

- Dedicated application DB + file DB architecture
- MongoDB browser
- BSON-aware document editor
- Aggregation builder
- Explain plans
- Index management
- Read-only server enforcement

### 🔥 Telegram

- Auto-filter
- Advanced result filtering
- Multi-page selection
- Send All
- force subscribe
- request-to-join force subscribe
- authorization/sudo
- broadcast
- user settings
- token access
- configurable command suffix

### 🔥 Media

- HTTP streaming
- direct download
- secure hash-based links
- multi-client support
- media information extraction
- external file-host integrations
- configurable shorteners

### 🔥 Metadata & Discovery

- TMDb metadata
- customizable IMDb/TMDb captions
- language/quality/season/episode/year extraction
- multiple external scraper sources
- Telegram search + web search

---

# 🧪 Troubleshooting

<details>
<summary><b>❌ Port already in use</b></summary>

Check:

```bash
ss -ltnp 'sport = :8888'
```

Replace `8888` with your configured `PORT`.

Also check:

```bash
pgrep -af "[-]m bot"
docker compose ps
```

Make sure only one component owns the bot process.

</details>

<details>
<summary><b>🔒 Duplicate bot / database locked</b></summary>

The Telegram session can be affected by multiple bot instances.

Check:

```bash
pgrep -af "[-]m bot"
```

Use the project's single-instance mechanism and avoid simultaneously running:

- a manually started bot
- a panel-supervised bot
- another Docker bot container

</details>

<details>
<summary><b>🔄 Setting saved but not applied</b></summary>

Check whether the variable is hot-reloadable.

Some values are inherently startup-bound, such as values associated with:

- Telegram client initialization
- bound network sockets
- startup-time handler registration

The panel reports when a restart is required.

</details>

<details>
<summary><b>🌐 Panel inaccessible</b></summary>

Check:

```bash
docker compose ps
ss -ltnp
```

Also verify:

```env
PANEL_PORT=
PANEL_HTTPS=
PANEL_IP_ALLOWLIST=
ADMIN_USERNAME=
ADMIN_PASSWORD=
```

If HTTPS mode is enabled while serving plain HTTP, secure cookies will not behave as expected.

</details>

<details>
<summary><b>🗃️ MongoDB storage/quota problems</b></summary>

Check:

```text
DB_STORAGE_LIMIT_MB
SWITCH_BUFFER_MB
FILES_DATABASE_URL
```

For multiple file databases, make sure the configured URLs are valid and accessible from the host.

A larger switch buffer gives the indexer more safety margin before moving to another database.

</details>

<details>
<summary><b>🔀 Wrong branch / code unexpectedly replaced</b></summary>

The update mechanism can use:

```env
UPSTREAM_REPO=
UPSTREAM_BRANCH=
```

Review both configuration and database-backed settings before deployment.

**Always commit important project changes before allowing an automated hard reset/update workflow to run.**

</details>

---

# ⚠️ Important Operational Notes

### 1. 🔐 Protect credentials

This repository is intended to be configured through environment/configuration files. Do not publish live credentials.

### 2. 🗃️ MongoDB is an external dependency

The bot expects accessible MongoDB services. Database availability, network access, credentials and provider limits remain deployment concerns.

### 3. 🌐 Streaming requires network reachability

`BOT_BASE_URL` must resolve to a reachable HTTP endpoint and the configured `PORT` must be exposed through your hosting/network configuration.

### 4. 🧱 The panel is intentionally independent

Do not merge the panel into the bot process casually. Its separation is part of the architecture and allows it to remain useful when the bot itself fails.

### 5. 🔄 Update workflow can replace uncommitted code

If an automated update script performs a hard reset, uncommitted project files can be lost.

### 6. 🧪 External scraper sources can change

Scrapers depend on third-party site structures and configured URLs. A source changing its HTML/API can require a scraper update.

---

# 📸 Screenshots / Demo

Add your preferred project screenshots or GIFs here:

```text
docs/
├── bot-search.png
├── select-mode.png
├── web-search.png
├── admin-dashboard.png
├── mongo-manager.png
├── database-status.png
└── live-reload.gif
```

Recommended showcase order:

1. 🤖 Telegram Auto-Filter result
2. ☑️ Multi-page Select Mode
3. 🌐 Web Search
4. 🛡️ Admin Dashboard
5. 🍃 MongoDB Manager
6. 🗃️ Multi-Database Status
7. ♻️ Live Config Reload
8. 📊 Process/host monitoring

---

# 🌟 SHOW-OFF SUMMARY

<div align="center">

### **IRON-FILTER-BOT**

**Search. Index. Filter. Stream. Manage. Scale.**

| 🧩 | Capability |
|---|---|
| 🤖 | Telegram Auto-Filter |
| 🔎 | Advanced metadata search |
| ☑️ | Multi-page file selection |
| ⚡ | Concurrent indexing |
| 🗃️ | Multi-Mongo file storage |
| 🔄 | Automatic DB switching |
| 🎬 | TMDb/IMDb metadata |
| 🕷️ | Multi-source scraping |
| 🌐 | Web search |
| 🎥 | HTTP streaming/download |
| 🔐 | Secure file links |
| 🛡️ | Standalone Admin Panel |
| ♻️ | Live configuration reload |
| 📊 | Process & host monitoring |
| 🍃 | MongoDB Manager |
| 🧪 | Aggregation / explain / indexes |
| 🔢 | TOTP 2FA |
| 🧾 | Audit logging |
| 💾 | Config backup/restore |
| 🔁 | Crash supervision/restart |

### 🚀 **Built for developers who want more than a basic Auto-Filter Bot.**

</div>

---

## 📜 License

Check the repository's license files/terms before redistributing or deploying modified versions.

---

<div align="center">

### ❤️ IRON-FILTER-BOT

**Made to be configured. Built to be extended. Designed to SHOW-OFF.**

</div>
