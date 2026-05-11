# Drumroll

A no-replacement random picker for meetings. Add a list of names or items, then pull each one from the hat in turn — with a synthesized drumroll sound, a big animated reveal, and a "last but not least" finale for the final pick.

Built in plain HTML/CSS/JS with the [Meadow](https://daymeadow.com) design language. Audio is synthesized live via the Web Audio API — no sound assets to host.

## Run locally

```bash
# any static server works
cd public && python3 -m http.server 8080
```

Then open <http://localhost:8080>.

## Deploy

Hosted on Firebase: <https://drumroll-app-3415.web.app>

```bash
firebase deploy --only hosting
```
