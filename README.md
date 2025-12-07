# Pac-Man Contribution Graph for sarcasticpanda

This repository auto-generates a Pac-Man animation that visualizes your GitHub contribution graph (streak). The workflow pushes the generated artifact into the `output` branch so you can embed the SVG in your profile README.

## What I changed
- Increased Node's V8 heap for the workflow to avoid OOM errors.
- Enabled `include_private: true` in the workflow and added instructions to supply a Personal Access Token (PAT) via the `PERSONAL_TOKEN` secret so private contributions (full streak) can be displayed.
- Increased workflow timeout to 15 minutes.

## Setup (what you need to do)
1. Create a Personal Access Token with `repo` scope (to allow reading private contributions). Go to Settings -&gt; Developer settings -&gt; Personal access tokens -&gt; Generate new token.
2. In this repository, go to Settings -&gt; Secrets and variables -&gt; Actions -&gt; New repository secret. Name it `PERSONAL_TOKEN` and paste the PAT. This is optional, but required if you want private contributions included.
3. Enable Actions if disabled. The workflow runs every 6 hours and on push to main.

## Embed the generated Pac-Man in your profile README
After the workflow runs successfully it commits the generated files to the `output` branch under `dist/`. You can embed the generated image in your README like this:

```markdown
![Pac-Man Contribution Graph](https://raw.githubusercontent.com/sarcasticpanda/sarcasticpanda/output/dist/pacman-contribution-graph.svg)
```

If you prefer to serve via GitHub Pages, configure Pages to serve from the `output` branch `/(root)` or update the `ghaction-github-pages` step accordingly.

## Troubleshooting
- If you see `FATAL ERROR: Ineffective mark-compacts near heap limit Allocation failed - JavaScript heap out of memory` in the Actions logs, the workflow now sets `NODE_OPTIONS=--max-old-space-size=4096`. If OOM persists, we can increase it to 8192.
- If the animation still doesn't show your full streak, ensure `PERSONAL_TOKEN` is set as described above.

## Notes about automation
I updated the workflow so it uses the `PERSONAL_TOKEN` secret if present; I cannot create or set your secrets or tokens on your behalf for security reasons. Please add the `PERSONAL_TOKEN` secret if you want private contributions included.
