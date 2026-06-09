# Stream Extractor GitHub Actions + GitHub Pages Runner

This repo converts the original PyQt desktop script into a GitHub-deployable setup:

- `extractor_core.py` — core extractor functions from the original Python file, without PyQt GUI imports.
- `stream_extractor_cli.py` — CLI runner for local use and GitHub Actions.
- `.github/workflows/extract.yml` — runs extraction manually or when a GitHub issue title contains `[extract]`.
- `.github/workflows/pages.yml` — deploys the static web page from `docs/`.
- `docs/index.html` — GitHub Pages UI that creates a prefilled issue to trigger the Action.

> Use this only for media you own or have permission to process. Do not use it to bypass DRM, paywalls, or access controls.

## How the webpage works

GitHub Pages is static. It cannot run Python by itself and it should not store a GitHub token in browser JavaScript. This project uses a safer pattern:

1. User opens the GitHub Pages webpage.
2. User pastes URLs.
3. The page opens a prefilled GitHub issue with title `[extract]`.
4. The `issues` trigger starts `.github/workflows/extract.yml`.
5. The workflow runs Python and comments the result on the issue.
6. The workflow also uploads `results.json` and `results.md` as an artifact.

## Deploy steps

1. Create a new GitHub repository.
2. Upload all files from this folder.
3. Go to **Settings → Pages**.
4. Set source to **GitHub Actions**.
5. Go to **Actions** and enable workflows if GitHub asks.
6. Run **Deploy GitHub Pages** once, or push to `main`.
7. Open your Pages URL and enter your owner/repo in the page.

## Manual GitHub Actions run

Go to **Actions → Run extractor → Run workflow** and paste URLs into the `urls` input.

## Local run

```bash
python -m pip install -r requirements.txt
python stream_extractor_cli.py --url "https://example.com/embed/abc123"
```

Or with a file:

```bash
python stream_extractor_cli.py --urls-file urls.txt --output-dir results
```

## Notes

- Public GitHub issues are public. Use a private repo for private URLs.
- Some hosts may block datacenter IPs, including GitHub-hosted runners.
- GitHub-hosted runners have time limits and changing IP ranges, so extraction that works locally may fail in Actions.
- For a true instant web app, use a backend host such as Render, Railway, Fly.io, or a VPS instead of GitHub Pages.
