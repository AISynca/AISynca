# AISynca

AISynca is a Chrome extension for sending one prompt to multiple AI platforms and comparing the responses side by side.

Supported AI platforms:

- ChatGPT
- Gemini
- Claude
- Grok

## Main Features

- Send one prompt to multiple AI platforms simultaneously
- Dashboard / Split / Window layout modes
- Use existing AI accounts, subscriptions, memories, and available models
- Premium features may include model selection, thinking/reasoning controls, file and image attachments, prompt templates, and AI-specific templates

## Directory Structure

```text
AISynca/
├── extension/          Chrome extension source files
├── docs/               GitHub Pages website
├── store-listing/      Chrome Web Store listing text and screenshots
├── README.md
├── CHANGELOG.md
├── LICENSE
└── .gitignore
```

## Chrome Extension Development Loading

1. Open `chrome://extensions` in Chrome.
2. Turn on **Developer mode**.
3. Click **Load unpacked**.
4. Select the `extension/` directory.
5. Confirm that `extension/manifest.json` exists before loading the extension.

> Note: The current repository may contain only a placeholder `extension/` directory until the real Chrome extension source files are added.

## GitHub Branch / Release Strategy

AISynca should be managed in a single GitHub repository. Public release code and active development code should be separated by branches, tags, and GitHub Releases, not by separate repositories.

### Branches

- `main`: stable branch. Only code ready for Chrome Web Store submission should be merged here.
- `develop`: active development branch for the next version.
- `feature/*`: feature-specific branches created from `develop`.
- `fix/*`: bug fix branches.
- `release/*`: pre-release stabilization branches before merging into `main`.

### Tags and GitHub Releases

- Create tags such as `v1.0.0`, `v1.1.0`, and `v1.2.0`.
- Each Chrome Web Store submission should correspond to a Git tag and GitHub Release.
- The `extension/manifest.json` version, `CHANGELOG.md` entry, Git tag, and GitHub Release version should match.

Example:

```text
extension/manifest.json: 1.1.0
Git tag: v1.1.0
GitHub Release: v1.1.0
```

Before publishing a new Chrome Web Store version:

1. Update `extension/manifest.json` version.
2. Update `CHANGELOG.md`.
3. Merge the release branch into `main`.
4. Create a Git tag, for example `v1.1.0`.
5. Create a GitHub Release.
6. Build the Chrome Web Store zip from the matching tag or `main` branch.

Do not commit Chrome Web Store submission ZIP files or `.crx` files.

## GitHub Pages

To publish the website from `docs/`:

1. Open the GitHub repository settings.
2. Go to **Pages**.
3. Set **Source** to **Deploy from a branch**.
4. Set **Branch** to `main`.
5. Set **Folder** to `/docs`.
6. Save.

## User Data Safety During Updates

New versions must not delete or overwrite user settings, saved prompt templates, or AI-specific templates.

Recommended storage keys:

- `aisynca_storage_version`
- `aisynca_settings`
- `aisynca_templates`
- `aisynca_ai_specific_templates`
- `aisynca_license_status`

Rules:

- Do not call `chrome.storage.local.clear()` or `chrome.storage.sync.clear()` without explicit user action.
- Do not overwrite existing templates with an empty array during updates.
- Do not overwrite existing settings with default settings during updates.
- Add new default setting fields by merging defaults with existing values.
- Use `chrome.runtime.onInstalled` and handle `install` and `update` separately.
- Use storage migration when key names or data formats change.

## Update Data Retention Test

Before publishing a new version:

1. Load the old version in Chrome.
2. Change settings.
3. Create multiple templates.
4. Create AI-specific templates.
5. Check Chrome storage values.
6. Replace the extension files with the new version.
7. Reload the extension.
8. Confirm settings remain.
9. Confirm templates remain.
10. Confirm AI-specific templates remain.
11. Confirm no migration errors appear in the console.
12. Confirm `aisynca_storage_version` is updated when migration runs.

## Security Notes

Repository visibility warning: this repository is private, but secrets must still never be committed.

Never commit:

- API keys
- Tokens
- Webhook secrets
- License validation secrets
- `.env` files
- Private keys
- Chrome Web Store `.crx` files
- Submission ZIP files
- Unpublished paid feature enforcement secrets

Use `.env.example` only for documenting required environment variable names.

## License

This project is proprietary. See `LICENSE` for details.
