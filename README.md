# ghostwriter-of-shame

**Commit Shame Arcade** - Commit Shame Arcade - an arcade cabinet that scores your Git commit messages like a video game. Paste commits or fetch a public GitHub repo.

![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)
![No Build](https://img.shields.io/badge/build-none-lightgrey.svg)
![Static](https://img.shields.io/badge/backend-none-brightgreen.svg)

**Live:** [imdarshangk.github.io/ghostwriter-of-shame](https://imdarshangk.github.io/ghostwriter-of-shame/)

## What it does

- **Paste mode** - drop in a list of commit messages, one per line, and press start.
- **GitHub repo mode** - enter `owner/repo` and it fetches the most recent public commits directly from the GitHub REST API, straight from your browser.
- Detects patterns like one-word commits (`fix`, `wip`, `asdf`), duplicate messages, "final final" energy, apology commits, and - when timestamps are available - late-night commits filed between 11pm and 5am.
- Each detected pattern lands as a scored **combo hit** (e.g. `GENERIC COMMIT -16pts`), tallied live under an arcade-style score counter that counts up from 000.
- Final score earns a badge: **HIGH SCORE**, **CONTINUE?**, or **GAME OVER**.
- Lets you copy the full results as plain text.

## Run it locally

No install, no build step:

```bash
git clone https://github.com/imdarshangk/ghostwriter-of-shame.git
cd ghostwriter-of-shame
```

Then open `index.html` directly, or serve it:

```bash
python3 -m http.server 8080
# or
npx serve .
```

Visit `http://localhost:8080`.

## How it works

Everything runs client-side in vanilla HTML, CSS, and JavaScript - no framework, no build tooling.

- Commit messages (pasted, or fetched from `api.github.com/repos/{owner}/{repo}/commits`) are scanned against word lists for generic phrasing, apologies, frustration, and "finality" language, plus checks for duplicates, message length, and commit hour.
- Each signal that fires becomes a combo hit with its own point deduction; the final score (0–100) is 100 minus every hit, floored at 3 and capped at 98.
- The score counts up on screen, hits reveal in sequence, and a badge locks in based on the final number: 70+ is HIGH SCORE, 40–69 is CONTINUE?, below 40 is GAME OVER.
- In repo mode, the fetch goes directly from your browser to GitHub's public, unauthenticated API - nothing is sent to, stored by, or proxied through anything we run. GitHub's anonymous rate limit is 60 requests/hour per IP; if you hit it, switch to paste mode.

## Deploying your own copy

Single static `index.html`, deploys anywhere that serves static files:

- **GitHub Pages:** repo Settings → Pages → Deploy from branch `main`, folder `/ (root)`.
- **Netlify:** Add new site → Import an existing project → point at this repo → leave the build command empty, publish directory `.`.
- **Anywhere else:** drag the folder onto any static host.

## Contributing

PRs welcome. The word lists (`GENERIC_WORDS`, `APOLOGY_WORDS`, `FRUSTRATION_WORDS`, `FINALITY_WORDS`) are near the top of the `<script>` block in `index.html`. New combo types can be added inside the `evaluate()` function by calling `ded('YOUR LABEL', 'detail text', pointsToDeduct)`.

## License

MIT. See [LICENSE](LICENSE).
