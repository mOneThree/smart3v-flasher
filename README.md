# Smart 3V board setup page

Live at **https://flasher.simplysmart.me/**

A single web page that installs the Smart 3V firmware on a new board over USB, shows the board's ID and
Matter pairing code, and keeps a list of set-up boards with printable labels. Works in Chrome or Edge on a
computer (it uses Web Serial). No install, no login. The board list is saved only in the browser that
set the boards up.

Firmware updates for boards that are already installed come from the Home Assistant add-on and the
`smart3v-updates` repo, not from here.

## Files

| File | What it is |
|---|---|
| `index.html` | The page (one file, no build step) |
| `boards.json` | The board types the page offers, and which firmware file each one uses |
| `firmware/` | Full USB images, written at address 0 |
| `lang/en.json` | Every piece of text on the page, in English |
| `lang/languages.json` | The languages the page offers |
| `vendor/esptool-js-0.7.1.js` | Espressif's esptool-js, Apache-2.0 (licence alongside) |
| `vendor/qrcode-generator-1.4.4.js` | QR code drawing for results and labels, MIT (Kazuhiko Arase) |
| `CNAME` | The custom domain for GitHub Pages. Do not delete |
| `.nojekyll` | Tells GitHub Pages to serve the files as they are |

## Adding a board type or a new firmware version

1. Put the board's full USB image (`...-usb-full.bin` from the firmware repo's `builds` branch) in `firmware/`.
2. In `boards.json`, set that board's `firmware` entry: `version`, `file`, `address` (0), `size` in bytes,
   and `sha256`. Get the last two with `sha256sum` and `stat -c %s` (Linux) or
   `Get-FileHash` and `(Get-Item file).Length` (PowerShell).
3. `chip` must match what esptool-js reports, for example `ESP32-C6`.
4. Set `status` to `ready` only after the board has been set up with the page once. Otherwise leave it `planned`.

Every different hardware type needs its own firmware build and its own Matter product ID, so Home Assistant
never offers a board the wrong update.

## Looks and languages

The page has three looks (Workbench, Friendly, Minimal), each in light and dark. The visitor's choice is
remembered in their browser. All colours, fonts and shapes are CSS variables at the top of `index.html`,
one block per look and mode.

To add a language: copy `lang/en.json` to `lang/<code>.json` (for example `lang/nl.json`), translate the
values (keep the keys and the `{placeholders}`), then add `{ "code": "nl", "name": "Nederlands", "dir": "ltr" }`
to `lang/languages.json`. A language picker appears as soon as there are two languages. Any missing key falls
back to English. Use `"dir": "rtl"` for right-to-left languages.

When you change the page, raise `PAGE_VERSION` and `data-page-version` in `index.html`.

## Testing without the website

Run `python -m http.server 8000` in this folder and open http://localhost:8000/ in Chrome.
