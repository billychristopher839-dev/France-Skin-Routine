# France : Road to #1 — Le Protocole Présidentiel

A single-file habit tracker. You are the President of France. Sign one skincare décret every evening; every 3 logged days a real French department pledges loyalty, in order from Paris outward. Log 285 days, rally all 95 departments, and France finishes #1 in world governance.

Fully bilingual (EN / FR), zero external dependencies at load time, works offline.

## What's inside

- **🏛️ Élysée** — daily décret, live stats (points, streak, world rank, press standing), the interactive department map with real facts per department, and three mini-games in La Salle des Fêtes (+1 bonus each per day).
- **📅 Journal** — a month calendar of your whole mandate. Green = full protocol, amber = night cream only, red = negligence, grey = no décret. Gold dots mark days with game bonuses. Monthly totals underneath.
- **📖 Guide** — the objective, scoring, unlock rules, and cloud setup, in both languages.
- **👤 Profile** — your name and official form of address (used in the daily briefing), language, and the cloud archives card.
- **Onboarding** — a three-step briefing on first launch: language, your papers, the rules of the mandate.

## Storage

Offline-first, three-tier fallback:

1. `window.storage` (Claude artifacts environment)
2. `localStorage` (GitHub Pages / any browser, including local files)
3. In-memory (last resort; survives the session only)

Key: `fr_pres_v1`. Existing v1 saves migrate automatically.

## Cloud sync (optional): Firebase Realtime Database + Google sign-in

The Firebase SDK loads dynamically only when you use it. Without configuration, the game simply runs local-only.

Setup (~2 minutes):

1. Create a project at [console.firebase.google.com](https://console.firebase.google.com) and add a **Web app**.
2. Copy the web app's config into `FIREBASE_CONFIG` near the top of the `<script>` in `index.html`.
3. **Authentication → Sign-in method** → enable **Google**.
4. Create a **Realtime Database**, then add your GitHub Pages domain under **Authentication → Authorized domains**.
5. Recommended database rules:

```json
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "$uid === auth.uid",
        ".write": "$uid === auth.uid"
      }
    }
  }
}
```

Then sign in from the **Profile** tab. On sign-in, cloud and local décrets are merged (union of days; the device you're holding wins any conflict), and every save afterwards backs up automatically.

Note: Google's sign-in popup requires a real domain — it works on GitHub Pages but not from a `file://` page. Everything else works everywhere.

## Deploy to GitHub Pages

1. Create a repo (e.g. `france-road-to-1`) and push this folder's contents.
2. Settings → Pages → deploy from the `main` branch, root.
3. Visit `https://<username>.github.io/france-road-to-1/` and hard-refresh (⌘⇧R) after each deploy.

`.nojekyll` is included so Pages serves the file untouched.

## Testing

The game logic lives in a pure IIFE between `GAME_LOGIC_START` / `GAME_LOGIC_END` markers, so it can be extracted and exercised headlessly:

- `node --check` on the extracted script (syntax)
- Unit tests: logging, scoring, streaks, migration, state merge (13 tests)
- jsdom integration: onboarding flow, tabs, calendar rendering and navigation, profile persistence, localStorage fallback, reload restore, FR/EN i18n (29 tests)

Liberté, Égalité, Hydratation.
