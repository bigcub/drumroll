# AGENTS.md

Guidance for coding agents (Claude Code, Cursor, Codex, Aider, etc.) working on this repo.

## What this is

Drumroll is a single-file vanilla HTML/CSS/JS app — a no-replacement random picker with a synthesized drumroll sound and animated reveal. **No framework, no build step, no npm packages.**

## File layout

```
public/index.html    The whole app — HTML + CSS in <style> + JS in <script>
public/favicon.svg   Snare-drum SVG favicon
firebase.json        Firebase Hosting config
.firebaserc          Points at the original Firebase project (you can't deploy there)
README.md            Human onboarding, including team workflow
AGENTS.md            This file
```

When you change behaviour, you are almost always editing `public/index.html`. There is nothing else.

## Conventions to keep

- **Stay vanilla.** No React, Vue, Tailwind, Vite, npm dependencies, or bundlers. Do not introduce a build step. If you find yourself wanting one, stop and ask the user first.
- **Single file.** Keep CSS in `<style>` and JS in `<script>` inside `public/index.html`. Don't split into separate files without an explicit request.
- **Meadow design language.** Lora for display/headings, DM Sans for UI. Use the CSS variables already defined in `:root` and `[data-theme="dark"]` (`--bg`, `--surface`, `--text`, `--accent`, etc.) — don't hardcode colours. 18px card radius, soft warm shadows, generous padding. **Never** introduce blue-grey tech palettes, neon accents, loud gradients, or sharp corners.
- **Both themes.** Light and dark are first-class. Every visual change must look good in both. Test by toggling the sun/moon button.
- **Audio is synthesized.** Sounds are built with the Web Audio API from `OscillatorNode` + filtered noise buffers. Do not add `.mp3` / `.wav` files or external audio dependencies.
- **State persists in `localStorage`** under the key `drumroll-state-v1`. If you change the shape of stored state, bump the version (`v2`, `v3`) and handle migration — otherwise users will hit cryptic bugs from stale data.

## How to run and test

```bash
cd public && python3 -m http.server 8080
# → http://localhost:8080
```

There are no automated tests. **You must manually verify in the browser** after any change. The picking flow has had real bugs that only show up when you actually click through it, so this matters.

### Required manual checks before declaring a change done

1. Add a few names. They appear in the list.
2. Click **Pick next**. The drumroll plays, a name is revealed big in the middle, and the same name (not a different one) is crossed out in the sidebar.
3. Repeat until only one name remains. The final pick triggers the gold "Last but not least…" finale (longer roll, brass fanfare, sparkles, gold confetti).
4. Refresh the page. State persists.
5. Click **Reset**. All picks are cleared; items stay.
6. Click **Clear**. Items are removed.
7. Toggle the theme. Both light and dark look correct.
8. Open devtools — no console errors.
9. `localStorage.clear()` once, then re-run step 1–4. Stale state can mask bugs (this has caught a real "false positive" before — the user thought a bug was present when in fact it was only old data from a previous version).

## Things to be careful of

- **Picking flow.** The winner must be chosen up-front at click time and held in a closure variable; the shuffle animation must not overwrite the final reveal. There is a `stepActive` kill-switch and a real-time elapsed check — preserve them.
- **Duplicate names.** Items are distinguished by `id`, not `name`. The user can legitimately add the same name twice. Don't deduplicate by name.
- **Timing.** The roll duration and acceleration curve are tuned to match the audio. If you change one, retune the other.

## Out of scope without asking

- Changing the Firebase project, `firebase.json`, or `.firebaserc`
- Adding any dependency, package manager, or build tool
- Switching to a framework
- Splitting `public/index.html` into multiple files
- Adding analytics, tracking, or third-party scripts
- Rewriting the design language

## Git

- Don't commit on `main`. Work on a branch named after the change.
- Commit messages: imperative mood, focus on the why ("Fix winner/display mismatch on rapid clicks"), not the what ("Edit index.html").
- Don't `git push --force` unless the user explicitly asks.
- Don't change `git config` user identity.
