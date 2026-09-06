# Brand assets — Portfolio Calendar Pull

## Originality and copyright

The logo is **original work**, created for this project. It carries no copyright or
trademark risk:

- It is generated entirely by `make_logo.py` from primitive geometry — rounded
  rectangles, one rectangle and one triangle, plus a two-stop linear gradient.
- No stock art, no icon pack, no traced or derived third-party image, no font glyphs,
  no AI-generated image asset, no external file of any kind is used.
- Re-running `python3 brand/make_logo.py` reproduces every file byte-for-byte, so the
  provenance of the mark is fully auditable in this repository.
- The design is a generic calendar outline with a downward arrow. Simple geometric
  shapes and generic iconographic concepts are not protectable by copyright, and the
  mark does not imitate the trade dress, colour scheme or layout of Google Calendar or
  any other product.

Copyright © 2026 Fernando Alarcón. All rights reserved.

## Files

| File | Size | Use |
|---|---|---|
| `logo-512.png` | 512×512 | Master / high-resolution source |
| `logo-240.png` | 240×240 | Website header (copied to `docs/logo.png`) |
| `logo-120.png` | 120×120 | **Upload this one to the OAuth consent screen** |

Google's Branding page requires: square, PNG/JPG/BMP, under 1 MB, 120×120 recommended.
`logo-120.png` is 5 KB.

## Regenerating

    cd ~/Documents/portfolio-calendar-pull-website
    python3 brand/make_logo.py

Requires Pillow (`pip3 install Pillow`). The script also refreshes `docs/logo.png` and
`docs/favicon.png`, so the consent screen and the website always show the same mark —
which is what Google's reviewers check.

## Design

- Gradient `#5B7CFF` → `#1E2FA8`, top to bottom.
- White calendar frame with two hanging tabs.
- Downward arrow = "pull" — the app pulls events out of your calendar.
