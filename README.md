# Hexy's Bot Manager

A desktop dashboard (Tkinter) for running and managing a Discord bot. Toggle
features on and off, watch live logs, browse your server's channels and
voice chats in real time, and moderate members, all from one window.

![Python](https://img.shields.io/badge/python-3.10%2B-blue)
![License: MIT](https://img.shields.io/badge/license-MIT-green)

## Features

- **Plugin system.** Toggle bot features on and off from the UI. Ships with:
  - `/math`, an expression evaluator with unit conversions and constants
  - `/weather`, live weather lookup for any city, with autocomplete
  - An example plugin template so you can write your own
- **Live dashboard.** Connection status, ping, uptime, and per-plugin PID/status
- **Debug mode.** Pipe any plugin's stdout/stderr straight into the log,
  with automatic error/warning highlighting
- **Servers view.** See every server the bot is in, with icons and member counts
- **Chat browser.** Expand every category and channel in a server. Open a
  text channel to watch its live chat, or a voice channel to see who's
  connected. Both update in real time, no refresh needed
- **Moderation.** Click any username to Kick, Timeout, or Ban them
  directly, with a duration picker for timeouts and temporary bans
- **Persistent temp bans.** A temporary ban's exact expiry (full date and
  time, not just a clock time) is saved to disk. If the app is closed and
  reopened later, it still unbans at the right moment instead of getting
  confused by day changes
- **Send messages.** Post a staff message to any channel straight from the
  chat viewer
- **Lockdown mode.** A panic button that blocks all bot commands and
  auto-deletes anything the bot tries to send

## Requirements

- Python 3.10 or newer
- A Discord bot application, created at the
  [Discord Developer Portal](https://discord.com/developers/applications)

## Setup

1. **Clone or download this repo.**

2. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Create a Discord bot** at the
   [Developer Portal](https://discord.com/developers/applications):
   - New Application, then Bot, then Reset Token, and copy it somewhere safe
   - Under Privileged Gateway Intents, enable:
     - Server Members Intent
     - Message Content Intent
   - Under OAuth2, URL Generator, check `bot` and `applications.commands`,
     then give it at minimum: Kick Members, Ban Members, Moderate Members,
     Send Messages, Read Message History, View Channels, Connect
   - Use the generated URL to invite the bot to your server

4. **Run the app:**
   ```bash
   python hexys_bot_manager.py
   ```
   Paste your bot token into the connect screen. The app creates its own
   `hexy_config.json`, `logs/`, `plugins/`, and `assets/` folders on first
   launch, all git ignored so your token never gets committed.

## Adding your own plugin

Every plugin is a standalone Python script, launched as:

```
python your_plugin.py --token BOT_TOKEN --state on|off
```

1. Copy `example_plugin.py` as a starting point.
2. Write your feature logic in `run_feature()`.
3. Register it in `hexys_bot_manager.py` by adding an entry to the
   `FEATURES` list near the top of the file:

   ```python
   {
       "name":        "My Feature",
       "description": "What it does.",
       "script":      "my_plugin.py",
       "builtin":     False,
   },
   ```
4. Restart the manager. A new toggle card appears automatically.

## A note on temporary bans

Discord itself doesn't support temporary bans natively, unlike timeouts,
which Discord's own servers expire automatically even if this app isn't
running. A temp ban set through this manager only auto-expires while the
manager process is running. If you close it before the ban is due to lift,
the ban stays in place until you reopen the app, at which point it picks
the countdown back up from the real stored expiry time, or until you unban
the person manually.

## Data and privacy

Everything the app stores lives in plain files right next to the script,
on your own machine, and nowhere else:

| File | What it holds |
|---|---|
| `hexy_config.json` | Your bot token and the last launch time |
| `mod_schedule.json` | Pending temporary ban records and when they expire |
| `math_state.json`, `weather_state.json` | Whether each plugin was left on or off |
| `*.log` | A running text history of what each plugin printed |
| `*.pid` | A process ID number, used to stop a plugin from starting twice |

Your bot token is only ever sent to Discord's own servers, since that's
simply what logging a bot in requires. The Weather plugin also calls
Open-Meteo's free geocoding and forecast API when someone uses `/weather`,
sending only the city text typed in and the coordinates that come back. No
API key or account is involved. Nothing else leaves your machine: no
analytics, no telemetry, and nothing reporting back to whoever wrote this
app.

Treat your bot token like a password. If it's ever exposed, regenerate it
right away from the Discord Developer Portal.

## License

MIT, see [LICENSE](LICENSE). Feel free to swap this out if you'd rather
use a different license for your fork.
