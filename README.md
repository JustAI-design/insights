# Insights Studio

An iPad-friendly page that reads your Project Management Insights episodes from Notion, builds the Claude prompt, turns the narration Claude writes into audio with Gemini text-to-speech, and gives you the title, description, date and audio file for Spotify for Creators.

Flow: pick an episode, copy the prompt into Claude, paste the narration back, generate audio, download it, then enter the four items in Spotify.

## Files

- `index.html` is the whole app. Host it on GitHub Pages.
- `worker/worker.js` and `worker/wrangler.toml` are a small Cloudflare Worker that holds your Notion token and returns your unpublished episodes (read only).

## One-time setup

1. **Notion integration.** Go to notion.so/profile/integrations, create an internal integration, and copy its secret. Open the episodes database in Notion, choose the three dots, Connections, and add the integration.
2. **Cloudflare Worker.** Create a free Cloudflare account. In the dashboard choose Workers and Pages, Create, Hello World, then replace the code with `worker/worker.js`. Under Settings add:
   - secret `NOTION_TOKEN` (the integration secret)
   - secret `PASSCODE` (any phrase you choose)
   - variable `ALLOWED_ORIGIN` set to `https://YOURNAME.github.io`
   Copy the worker URL it gives you.
3. **GitHub Pages.** Create a repository, upload `index.html`, then Settings, Pages, deploy from the main branch. Open the page URL on the iPad and use Share, Add to Home Screen.
4. **Gemini key.** Create a key at aistudio.google.com. On the iPad open Settings in the app and enter the worker URL, passcode and Gemini key. They stay in that browser only.

## Notes

- The prompt is the Script Prompt from Notion. If it contains the placeholder `<episode title + description>`, that is replaced by the title, description and key concepts. Otherwise the three are appended after it.
- Unpublished episodes only are listed. Ideas, Scripted and Recorded all appear.
- Long scripts are sent to Gemini in parts and joined into one file. MP3 is the default format, WAV is available.
- The voice is fixed to Kore. Change `CONFIG.voice` at the top of the script to use another.
- If Google renames the TTS model, change it in Settings. The page also tries two fallback models.
- Generated audio is kept in the browser until you clear it, so reloading the page does not lose it.
