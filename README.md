# NEXA AI — Full Netlify Project

A premium dark AI workspace with chat, provider fallback, image generation, realtime voice, weather lookup, and developer/community commands.

## Included
- `index.html` — app layout
- `assets/style.css` — responsive premium interface
- `assets/app.js` — chat, image, weather, commands
- `assets/realtime-voice.js` — browser WebRTC voice client
- `netlify/functions/chat.js` — Gemini → Groq → OpenAI fallback
- `netlify/functions/image.js` — OpenAI image generation
- `netlify/functions/realtime-token.js` — short-lived Realtime client secret
- `netlify.toml` — Netlify configuration

## 1. Configure API keys securely
In Netlify: Site configuration → Environment variables. Add whichever providers you use:
- `GEMINI_API_KEY`
- `GROQ_API_KEY`
- `OPENAI_API_KEY`

Optional:
- `OPENAI_CHAT_MODEL` (default `gpt-4o-mini`)
- `GROQ_MODEL` (default `llama-3.3-70b-versatile`)

Chat fallback order is Gemini, then Groq, then OpenAI. Image generation and realtime voice use OpenAI. Keys must only exist in Netlify environment variables, never in `assets/*.js`, HTML, or GitHub.

## 2. Deploy
1. Extract this ZIP.
2. Push the project folder to a GitHub repository.
3. In Netlify choose **Add new site → Import an existing project**.
4. Connect the repository. Build command can be blank; publish directory is `.`.
5. Add environment variables, then deploy.
6. After changing environment variables, trigger a fresh deploy.

You can also test locally with Netlify CLI:
```bash
npm install -g netlify-cli
netlify dev
```
Open the local URL shown by Netlify CLI. Do not open `index.html` directly because Functions require the Netlify dev server.

## 3. Add your Discord invite
Open `assets/app.js` and replace:
```js
window.NEXA_CONFIG={discordInvite:''};
```
with your real invite URL, e.g.:
```js
window.NEXA_CONFIG={discordInvite:'https://discord.gg/YOUR_INVITE'};
```
The `/discord` command opens this URL.

## 4. Developer command
The `/developer` command currently introduces MD Riyad Ahammed with the project profile included in the app. Edit the text in `runCommand()` in `assets/app.js` if you want different details.

## 5. Voice requirements
- Deploy to HTTPS (Netlify provides HTTPS).
- Allow microphone permission.
- Realtime API access and billing must be enabled for the OpenAI account.
- Browser support for WebRTC is required.
- Realtime model/API availability can change; if OpenAI changes the Realtime endpoint or model identifier, update `netlify/functions/realtime-token.js` and `assets/realtime-voice.js` to match current API docs.

## 6. Weather and time
Weather uses Open-Meteo geocoding and forecast endpoints, with no API key required. A live clock uses the visitor's device-local time. Weather needs internet access.

## 7. Important production security
This is a starter project, not a hardened public service. Before sharing it widely:
- Add rate limiting / abuse protection to Netlify Functions (for example, a verified CAPTCHA or authenticated user/session limits).
- Set spending limits and usage alerts with each AI provider.
- Consider restricting allowed origins and request sizes.
- Do not commit `.env` files or API keys.
- Never expose a provider secret in client-side JavaScript.
- Review provider pricing and terms before enabling image/voice endpoints.

## 8. Troubleshooting
- `404` on functions: use `netlify dev` or deploy through Netlify; don't open the HTML with `file://`.
- No chat provider configured: add at least one AI key in Netlify.
- Image/voice errors: verify OpenAI key permissions, billing, and model availability.
- Microphone unavailable: use HTTPS and allow microphone access.
