# Changelog

All notable changes to this project are documented in this file.

## [1.1.0] – 2025‑04‑16

### 🎉 Highlights
- **New `/quotacheck` command** to verify whether a user has reached the 4 hour quota.
- **Times now formatted in EST** (America/New_York) everywhere.
- **Approval flow enhancements**:  
  - **Approve** and **Deny** buttons always require a reason  
  - DMs now include who approved/denied, clock‑out timestamp, and duration
- **Automated slash‑command registration** on every deploy.
- **Secure Google Sheets integration** via Base64‑encoded credentials—no raw key file in repo.
- **Maintenance alerts & dynamic presence** with BetterStack heartbeats.

---

### 🆕 Added
- **`/quotacheck discordid:<ID>`**  
  Fetches total logged time from Sheets (range `R12:R122`), compares to `04:00:00`, and replies:  
  > “User [mention] has **HH:MM:SS** logged time, they have (NOT) completed quota.”
- **`deploy‑commands.js`** script  
  Registers `/quotacheck`, `/clockin`, and `/clockout` via the Discord REST API.
- **`@discordjs/rest`** & **`discord-api-types`** dependencies  
  Enables safe, programmatic upsert of slash commands.
- **`GOOGLE_CREDS_BASE64`** support  
  Decodes your service‑account JSON at startup into `credentials.json`; no file check‑in.

### 🔄 Changed
- **EST time zone** everywhere:  
  All `.toLocaleString()` and `.toLocaleTimeString()` calls now use `timeZone: 'America/New_York'`.
- **Clock‑in/out commands**  
  - `/clockin` → “🟢 Clocked IN at HH:MM:SS EST”  
  - `/clockout` → calculates duration, submits embed with Started/Stopped/Duration (EST)
- **Approval embeds & DMs**  
  - Footer now shows “Approved by …” or “Denied by …”  
  - DMs include:  
    ```
    ✅ Your clock‑out was APPROVED by USER#1234.
    • Clocked out: 3/14/2025, 05:12 PM EST  
    • Duration: 02:45:30
    ```
- **Deny flow** now always pops a “Reason” modal.

### ⚙️ Maintenance & Presence
- **BetterStack heartbeat** every 60 sec.  
- **Maintenance alerts**:  
  - On **enter** Maintenance → yellow embed in channel `1353567659509157968`.  
  - On **exit** Maintenance → green “back up and operational” embed.  
- **Bot presence**:  
  - Idle + “Undergoing maintenance!” during maintenance  
  - Online + “Watching over X deputies” (reads `P16` of member sheet) otherwise

### 🐛 Fixed
- Resolved `npm ci` lockfile mismatch by regenerating `package-lock.json`.  
- Stray quotes in `GOOGLE_APPLICATION_CREDENTIALS` are now stripped automatically.
