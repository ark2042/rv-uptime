# rv-uptime

External uptime monitor for [regimevault.com](https://regimevault.com), running entirely on GitHub Actions (no dependency on the monitored server).

## How it works

- A scheduled workflow (`.github/workflows/uptime.yml`) runs every 10 minutes.
- It fetches `https://regimevault.com/api/health` (3 attempts, 30 s apart) from GitHub runners.
- Site state (`up`/`down`) is persisted in the `state` file, committed on each transition.
- On a state **transition** (up→down or down→up), a Telegram alert is sent to the ops group. No spam while the state is stable.
- A monthly keepalive commit prevents GitHub from auto-disabling the scheduled workflow after 60 days of repo inactivity.

## Configuration

Repository secrets:

| Secret | Purpose |
|---|---|
| `TG_BOT_TOKEN` | Telegram bot token used to send alerts |
| `TG_CHAT_ID` | Target Telegram chat id |

## Notes

- Detection latency: up to ~10-15 min (cron granularity + GitHub scheduler lag). Complemented by an UptimeRobot monitor (5 min checks, email alerts).
- This repo contains no sensitive data: the monitored URL is the public website.
