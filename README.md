# 5ivemor — Lua Protection + Discord Bot + Web Panel

**Pre-configured for you:**
- Client ID: `1524325180678344724`
- Owner / Admin User ID: `1511253064567357561`


Full-stack system: Discord bot (slash commands, panels, keys) + Express web panel (login, scripts, obfuscator) + PostgreSQL.

## Quick Start (Local)

### 1. Requirements
- Node.js 18+
- PostgreSQL database (local or free cloud: [Neon](https://neon.tech), [Supabase](https://supabase.com), [Railway](https://railway.app))

### 2. Discord Application
1. Go to https://discord.com/developers/applications → **New Application**
2. **Bot** tab → Add Bot → Copy **Token** → put in `BOT_TOKEN`
3. **OAuth2** tab → Copy **Client ID** and **Client Secret**
4. Add Redirect URL: `http://localhost:3000/auth/discord/callback` (or your real domain later)
5. Invite the bot: OAuth2 → URL Generator → scopes `bot` + `applications.commands` → generate link → open it and invite to your server

### 3. Setup
```bash
cp .env.example .env
# Edit .env and fill in all values
npm install
npm start
```

- Website: http://localhost:3000
- Bot will come online and register slash commands

### 4. Important env vars
| Variable | Description |
|----------|-------------|
| `BOT_TOKEN` | Discord bot token |
| `API_SECRET` | Shared secret (bot ↔ server). Make it long & random |
| `API_BASE` | Public URL of the website (e.g. `https://your-app.up.railway.app`) |
| `DATABASE_URL` | Postgres connection string |
| `OWNER_ID` | Your Discord User ID (admin commands) |
| `DISCORD_CLIENT_ID` | Application Client ID |
| `DISCORD_CLIENT_SECRET` | Application Client Secret |
| `DISCORD_REDIRECT_URI` | Must match the one in Discord portal |

## Hosting the Website + Bot (Recommended: Railway)

Railway can run the Node app **and** provide PostgreSQL.

1. Create account at https://railway.app
2. **New Project** → **Deploy from GitHub** (push this folder to a repo) **or** upload
3. Add a **PostgreSQL** plugin → Railway gives you `DATABASE_URL` automatically
4. In Variables, set:
   - `BOT_TOKEN`
   - `API_SECRET` (generate a long random string)
   - `API_BASE` = your Railway public URL (e.g. `https://5ivemor-production.up.railway.app`)
   - `OWNER_ID`
   - `DISCORD_CLIENT_ID`
   - `DISCORD_CLIENT_SECRET`
   - `DISCORD_REDIRECT_URI` = `https://your-railway-url/auth/discord/callback`
   - `SESSION_SECRET` (another random string)
   - `PGSSL=true` (usually needed)
5. In Discord Developer Portal, update the Redirect URI to the Railway one
6. Deploy. Both `server.js` and `bot.js` start via `npm start`

### Alternative hosts
- **Render.com** — free tier available, add Postgres
- **Fly.io**
- Any VPS (DigitalOcean, Linode, etc.) with Node + Postgres

## Multi-bot support
Set `BOT_TOKEN_2`, `BOT_TOKEN_3`, etc. and matching `DISCORD_CLIENT_ID_2`… The same server can drive multiple bots.

## Notes
- The website is fully served by `server.js` (no separate frontend build).
- Static assets go in the `public/` folder.
- Owner-only commands (`/keypremium`, etc.) only work for the User ID in `OWNER_ID`.
- Never commit your real `.env` or bot token.
