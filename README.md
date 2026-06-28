# TubeFold — Chrome extension

Send the YouTube video you're watching to the [TubeFold](https://github.com/TubeFold/App)
desktop app for a Markdown summary.

The extension is a thin client: it talks to the locally-running TubeFold app over
`http://127.0.0.1:43821`. There is **no build step** — the source under `src/` ships
as-is.

**Versioning:** the extension version in `manifest.json` must always equal the
TubeFold app's `MARKETING_VERSION` (in [TubeFold/App](https://github.com/TubeFold/App)).
The publish workflow reads the app's version and **fails the release if they differ**,
so bump `manifest.json` to match whenever the app version changes. (Note: the Chrome
Web Store requires the version to strictly increase on every upload, so an
extension-only fix still needs a version bump — release it alongside the next app bump.)

## Develop

1. Clone this repo.
2. In Chrome: `chrome://extensions` → enable **Developer mode** → **Load unpacked** →
   select this folder.
3. Run the TubeFold app so the local API on port `43821` is up.

## Release

Publishing to the Chrome Web Store is automated — see
[`.github/workflows/publish.yml`](.github/workflows/publish.yml).

1. Bump `"version"` in [`manifest.json`](manifest.json).
2. Tag and push:

   ```sh
   git tag v0.5 && git push origin v0.5
   ```

The workflow zips the extension and uploads + publishes it via the Chrome Web Store API.

> **First publish must be manual.** The Web Store only issues an extension ID after the
> very first upload through the [Developer Dashboard](https://chrome.google.com/webstore/devconsole).
> Create the listing once by hand, then set the repo secrets below and every release
> after that is one `git push --tags`.

### Required repository secrets

| Secret | What it is |
|---|---|
| `CWS_CLIENT_ID` | OAuth client ID for the Chrome Web Store API |
| `CWS_CLIENT_SECRET` | OAuth client secret |
| `CWS_REFRESH_TOKEN` | OAuth refresh token (generated once) |
| `CWS_EXTENSION_ID` | the extension's ID, assigned at first manual upload |

Setup guide for the OAuth credentials:
<https://github.com/fregante/chrome-webstore-upload/blob/main/How%20to%20generate%20Google%20API%20keys.md>
