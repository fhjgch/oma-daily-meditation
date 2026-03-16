# 🙏 Oma Daily Meditation

Automatically sends Oma a different Shri Mataji speech on WhatsApp every morning.

Every day at 07:00 CET, a GitHub Actions workflow picks the next video from a playlist of 330 Shri Mataji talks, writes it to `today.json`, and commits it back to this repo. An iPhone Shortcut fetches that file and sends the YouTube link to Oma via WhatsApp.

---

## How it works

```
GitHub Actions (07:00 CET daily)
  → reads videos.json (330 talks)
  → reads state.json (tracks position)
  → writes today.json { title, youtube_url, date }
  → commits state.json + today.json back to repo

iPhone Shortcut (runs on automation, e.g. 08:00)
  → fetches today.json from raw GitHub URL
  → extracts youtube_url
  → sends "Genieße die heutige Rede von Shri Mataji 🙏 <url>" to Oma on WhatsApp
```

### Repository files

| File | Purpose |
|------|---------|
| `videos.json` | All 330 Shri Mataji talk URLs scraped from [amruta.org](https://amruta.org) |
| `state.json` | Tracks which video to send next (wraps around after 330) |
| `today.json` | Updated daily — contains the current day's title and YouTube URL |
| `.github/workflows/daily.yml` | GitHub Actions workflow that runs every morning |

---

## iPhone Shortcut setup

The shortcut is called **"Oma Daily Speech"** and looks like this:

<img width="700" height="667" alt="image" src="https://github.com/user-attachments/assets/24e129cd-c177-4c23-bc44-42e5a7cdd0b1" />


### Steps to recreate it

1. **Set Airplane Mode** — On *(forces WhatsApp to reconnect and deliver the message)*
2. **Wait** — 20 seconds
3. **Get contents of URL**
   ```
   https://raw.githubusercontent.com/fhjgch/oma-daily-meditation/main/today.json
   ```
4. **Get Dictionary Value** — Key: `youtube_url`, from: *Contents of URL*
5. **Send Message** via WhatsApp
   - Message: `[Dictionary Value] Genieße die heutige Rede von Shri Mataji 🙏`
   - Recipient: *Oma*
6. **Wait** — 30 seconds
7. **Set Airplane Mode** — Off

### Automate it

In the **Shortcuts** app → **Automation** tab → **+** → **Time of Day** → set to 08:00, every day → select this shortcut → disable *"Ask Before Running"*.

---

## Forking / adapting this for someone else

1. Fork this repo
2. Replace `videos.json` with your own playlist (same JSON structure: `date`, `title`, `city`, `youtube_url`, `page_url`)
3. Reset `state.json` to `{"index": 0}`
4. Update the raw GitHub URL in the shortcut to point to your fork
5. Update the recipient name in the shortcut

---

*Built with love for Oma. 🙏*
