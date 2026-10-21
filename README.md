# Bale Video Downloader (GitHub + Render Free)

## Deploy
1. Upload all repository files to GitHub, including `.github/workflows/broadcast.yml`.
2. In Render, create a **Blueprint** from this repository. It creates a Web Service.
3. Set `BOT_TOKEN` (Bale bot token) and `CRON_SECRET` (a long random secret) in Render.
4. In GitHub repository Settings → Secrets and variables → Actions, add:
   - `RENDER_URL`: the Render service URL, e.g. `https://your-service.onrender.com` (no trailing slash)
   - `CRON_SECRET`: exactly the same value as Render's `CRON_SECRET`
5. Start the bot in Bale with `/start`. The bot records private-chat recipients.
6. GitHub Actions runs the broadcast schedule (at 00:00, 10:00, 20:00 UTC) and calls Render. You can also run the workflow manually.

## Important free-tier limitations
- Render free Web Services can sleep after inactivity. A scheduled GitHub Actions request can wake the service, but wake-up can take time and scheduled jobs may be delayed.
- GitHub scheduled workflows are not exact-time guarantees and can be delayed or skipped under load. Therefore this free setup is best-effort, not a guaranteed exact 10-hour delivery service.
- Render's free filesystem is ephemeral. The recipient list is stored at `/tmp/bale_users.json`; a service restart/redeploy can erase it, so users may need to `/start` again. For durable recipient storage, use a persistent database (usually paid or an external service).
- Broadcast messages are sent only to users who initiated the bot in a private chat.
- Respect user consent and platform rules; provide an opt-out before using this for promotional messages.
