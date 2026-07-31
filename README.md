# Discord out-of-office auto-reply bot

Watches a **support channel** in every server the bot is in, and — **outside
your business hours** — replies once to each person with your out-of-office
message. One message + one schedule for all servers, all set via environment
variables.

## Files
```
index.js
package.json
.env.example
README.md
```

## Setup

### 1. Put the files in a GitHub repo
Create a new empty repo, then **Add file → Upload files** (or Create new file)
and add `index.js` and `package.json`. Commit.

### 2. Invite your bot to each client server
Use your bot's OAuth invite link (Discord Developer Portal → your app → OAuth2
→ URL Generator → scopes `bot`, permissions **View Channel** + **Send
Messages** + **Read Message History**). Open the link and add it to each client
server. (Administrator also covers this.)

### 3. Deploy on Railway
- New Project → Deploy from GitHub repo → pick this repo.
- Start Command: `npm start` (or leave default).
- **Variables** → set at least `DISCORD_BOT_TOKEN`, plus any of the settings in
  `.env.example` you want to change (message, hours, timezone, channel name).
- Check the logs for `OOO bot online as ...` and the line showing your hours
  and whether it's currently in or out of office.

## When it replies (`OOO_TRIGGER`)
- `mention` — only when your bot is **@mentioned** (least spammy)
- `channels` — any message in a **support** channel (`OOO_SUPPORT_CHANNELS`)
- `all` — **any** message in any channel (most aggressive)
- `both` — mention **or** support channel (**recommended default**)

## Anti-spam (built in)
- **One reply per person per out-of-office period.** No matter how many
  messages someone sends while you're out, they get a single "we're out"
  reply. When business hours resume, it resets — the next OOO period each
  person can get one again.
- A **cooldown floor** (`OOO_COOLDOWN_MINUTES`) as a secondary guard.
- Only ever replies **outside business hours**, and never to bots.

## Notes
- It replies in-channel, tagging the person, and stays silent during business
  hours.
- Matches support channels by **name** (default `support`) across all servers;
  set `OOO_SUPPORT_CHANNEL_IDS` for exact channels.
- No privileged intents needed — it doesn't read message text, it only reacts
  to a message arriving (or a mention) while you're out.
  
