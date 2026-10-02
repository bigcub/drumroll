# Drumroll

A no-replacement random picker for meetings. Add a list of names or items, then pull each one from the hat in turn — with a synthesized drumroll sound, a big animated reveal, and a "last but not least" finale for the final pick.

Built in plain HTML/CSS/JS with the [Meadow](https://daymeadow.com) design language. Audio is synthesized live via the Web Audio API — no sound assets to host.

Live at <https://drumroll.dej.app>.

## Run locally

```bash
# any static server works
cd public && python3 -m http.server 8080
```

Then open <http://localhost:8080>.

The whole app is a single file: `public/index.html` (HTML + CSS + JS inline). No build step, no dependencies.

---

## For the team — working on this app

This repo is a playground for extending Drumroll. You'll be using **git** and a **coding agent** (Claude Code, Cursor, etc.). Here's how to work on it without stepping on each other or breaking main.

### One-time setup

```bash
# 1. clone
git clone https://github.com/bigcub/drumroll.git
cd drumroll

# 2. tell git who you are (only needed once per machine)
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# 3. open it locally
cd public && python3 -m http.server 8080
# → http://localhost:8080
```

### The workflow for any change

Never edit `main` directly. Always work on a **branch**, then open a **pull request**.

```bash
# start fresh from main
git checkout main
git pull

# make a branch for your change — name it after what you're doing
git checkout -b add-shuffle-button

# … edit files, test in the browser …

# commit
git add -A
git commit -m "Add a shuffle button to reorder the list"

# push your branch
git push -u origin add-shuffle-button
```

Then go to GitHub and open a pull request from your branch into `main`. Someone reviews, then merges.

### Working with a coding agent

A few habits that make agents much more useful:

- **Tell it what you want, not how.** "Add a shuffle button that reorders the list randomly" beats "edit the render function to call a new function called shuffle that …".
- **Show it the bug.** Paste a screenshot or the exact wrong behaviour. "It's picking the same name twice" is what shipped the first real fix to this repo.
- **Run things yourself between turns.** Refresh the browser and watch the change. Agents are confident even when they're wrong — your eyes are the ground truth.
- **Stay on a branch.** Let the agent commit, but only push to your own branch. The PR review step catches a lot.
- **Small steps.** One change per branch. Easier to review, easier to revert.

### Deploying

The site is hosted on GitHub Pages. Every merge to `main` deploys `public/` to <https://drumroll.dej.app> automatically (see `.github/workflows/pages.yml`). There's no manual deploy step — merging the PR *is* shipping it.

To see your changes live before they're merged:

- **Easiest:** open the PR. The reviewer can preview locally.
- **Your own fork:** fork the repo, then in your fork go to **Settings → Pages** and set the source to **GitHub Actions**. Pushes to your fork's `main` will deploy to `https://<your-username>.github.io/drumroll/`.

### Ideas for things to add

- A "back" / undo button for the last pick
- Import names from a CSV or pasted spreadsheet column
- Categories or teams (pick one per team, round-robin)
- A countdown timer once each name is picked (for timed turns)
- Sound on/off toggle, volume control
- Export the pick order to clipboard

Pick one, branch off, and have a go.
