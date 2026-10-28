# PIT — disposable experiments, dependable publication

**Repository:** https://github.com/bonoj/Pit  
**Canonical Pages entry:** https://bonoj.github.io/Pit/  
**Branch:** `main`  
**Publisher:** [GitHub Actions](.github/workflows/pages.yml)

Pit is a **single-page mobile-first host** for small, independent browser experiments. It keeps build identity, deployment verification, FPS and refresh in a permanent shell. Experiments are isolated cartridges loaded into the remaining viewport via an iframe. No scrolling in the outer shell.

## Layout and cartridge contract

- `index.html` owns the header, build number, FPS counter, refresh button, status bar and `#stage`.
- `#stage` fills all remaining vertical space; `#cartridge` is an iframe at 100% width and height with no border.
- Place each experiment at `experiments/<slug>/index.html`, with its own assets under that directory. Do not modify the shell for ordinary experiment logic.
- Register the slug in the shell's `CATALOG` list. Open a cartridge using `?toy=<slug>`, or call `Pit.mount('<slug>')` from the host. The browser strips all query parameters and fragments from the visible address immediately after startup. This includes tracking parameters added by ChatGPT. **A clean URL does not encode the active cartridge**: a new visit to the clean root starts at the welcome screen.
- Each cartridge gets its own DOM, CSS and JavaScript context. A cartridge can be discarded by removing its directory and catalog entry. Avoid service workers and shared-origin persistent caches.
- The shell FPS measures **host requestAnimationFrame cadence**, not a cartridge's simulation FPS. A cartridge requiring precise simulation diagnostics must instrument its own frames.
- `voice` is an initial smoke-test cartridge; speech synthesis depends on browser support.

## Publication and build truth

The workflow publishes on every push to `main`. It packages a static site, replaces the single `__PIT_DEPLOY_SHA__` marker in the HTML with the exact commit SHA, writes `build.json` with the same SHA, deploys GitHub Pages, and polls the public HTML and manifest until they match.

The header reports the SHA **embedded in the executing HTML** (first eight characters). The status bar fetches `build.json` using `cache:'no-store'` and a unique request nonce; it reports VERIFIED only when the deployed SHA matches the executing HTML, otherwise STALE or UNVERIFIED. It checks on boot, tab reactivation, and every 15 seconds.

**Refresh** navigates to the shell with a unique `_pit_reload` parameter, forcing a distinct document URL. The startup script immediately removes all query parameters and fragments from the visible address using `history.replaceState`, so shared links remain canonical. This is a cache-busting request, not an assertion that all intermediary caches are infallible; the build comparison exposes mismatches.

The iframe source also carries a build parameter, ensuring a changed deployment requests a distinct cartridge URL. A browser tab can still hold old code until reloaded. Never claim an executing build based solely on a commit, Actions success, or a manifest fetch.

**Verification levels:** source committed → Actions successful → public entry and manifest checked → actual browser runtime observed. These are separate claims. Do not hand the human an unverified build number as if it were running.

## New cartridge recipe

1. Add `experiments/<slug>/index.html` (self-contained preferred).
2. Add `<slug>` to `CATALOG` in `index.html`.
3. Commit to `main`; publication triggers automatically.
4. Verify the workflow, public `build.json`, executing shell build ID, and cartridge behavior.
5. Use the clean canonical root URL in links. The `?toy=<slug>` selector is an optional entry mechanism; the shell strips it from the address bar on boot.

Keep Pit lightweight. No per-experiment deployment workflows, heavyweight framework, or directory noise at repository root.
