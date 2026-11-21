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
| `vendor/esptool-js-0.7.1.js` | Espressif's esptool-js, Apache-2.0 (licence alongside) |
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

## Testing without the website

Run `python -m http.server 8000` in this folder and open http://localhost:8000/ in Chrome.
