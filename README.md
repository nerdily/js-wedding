# J & S — Wedding Website

A single-page, password-protected, responsive wedding site for Bar Harbor, Maine · September 2026.

## Guest password

The guest password is **not stored in this repo on purpose** (per the spec: don't leave it in plain text anywhere public). Keep it somewhere private — a password manager, a note app — and share it with guests directly (save-the-date, invite insert, text).

Anyone without it sees only the welcome screen. The page content is **AES-256 encrypted** and is never present in plaintext in `index.html`, so it can't be read from "view source," and the password itself appears nowhere in the deployed files.

## Files

| File | Purpose |
|------|---------|
| `index.html` | **The deployable site.** Self-contained — just upload this one file. |
| `template.html` | The page shell (styling, password gate, decryptor). Edit for look & feel. |
| `build.js` | Holds all the **content** + map links. Encrypts it into `index.html`. |
| `claude.md` | Original requirements. |

## Changing content or the password

All wording, the agenda, addresses, and map links live in `build.js`. The guest password is passed in at build time (never hardcoded). After editing, rebuild:

```bash
node build.js "the-guest-password"
#   or
WEDDING_PW="the-guest-password" node build.js
```

This regenerates `index.html`. Re-deploy that file. (If you change the password, tell your guests the new one.)

## Deploying (serverless)

The site is one static file, so any static host works. Two easy free options:

### Cloudflare Pages
1. Push these files to a GitHub repo (or use Cloudflare's direct upload).
2. In the Cloudflare dashboard → **Workers & Pages → Create → Pages**.
3. Connect the repo (or drag-and-drop the folder). No build command needed; output directory is the project root.
4. Done — you get a `*.pages.dev` URL. Add a custom domain if you like.

### GitHub Pages
1. Push to a GitHub repo.
2. **Settings → Pages → Build and deployment → Source: Deploy from a branch**, pick `main` / root.
3. Your site appears at `https://<username>.github.io/<repo>/`.

> A `<meta name="robots" content="noindex, nofollow">` tag is already included so search engines won't index it.

## A note on the password protection

This is **client-side** encryption: strong enough that the content is unreadable without the password and safe from casual snooping or search engines — perfect for a wedding guest list. It is not bank-grade access control. If you ever want true server-enforced gating (e.g. revocable per-guest logins), put the site behind **Cloudflare Access**, which works with the same static files.
