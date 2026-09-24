# Snooker & Pool Layout Board

A top-down, to-scale snooker table and English 8-ball pool table in a single web page. You switch between them with the tabs at the top. Each table opens with the balls set for the break-off. Drag the balls to recreate any position, then share it as a link.

## Features

- **Two tables:** full-size snooker and 7 ft English pool (blackball). Each tab keeps its own position and undo history, so switching doesn't lose anything.
- The table, markings, balls and pocket jaws are drawn to scale in millimetres.
- Drag balls into place. Balls can't overlap and stay inside the cushions.
- Drag a ball off the table to pot it. Tap it in the tray to respot it, or drag it back onto the table. In pool, object balls respot on the black spot, or as close behind it as possible.
- **Snap** (on by default): a ball dropped close to a free spot, another ball or a cushion clicks into place on the spot, touching the ball or touching the cushion. Turn it off to set small gaps by hand.
- Undo, remove reds (snooker) or reds and yellows (pool), and reset to the break-off.
- Rotate the view. On phones the table picks whichever orientation makes the balls biggest.
- A status line shows the selected ball's position in mm from the baulk cushion and the left cushion, and what it's touching.
- On a keyboard, arrow keys nudge the selected ball by 1 mm, or 10 mm with Shift. Delete pots it.
- **Share layout** gives you a link to the current position. Links end in `#s1…` for snooker or `#p1…` for pool, and open on the right tab. A link ending `#pool` opens the pool table at the break-off. A plain link opens snooker at the break-off.

### On phones and tablets

- The table fills the screen. The game tabs are at the top, and Undo, Snap, Reset and More are docked at the bottom. More holds the other controls, sharing and the dimensions.
- While you drag, a magnifier above your finger shows the ball at 3.5× so your finger doesn't hide it.
- Dragging is geared: slow finger movements move the ball about a third as far, for fine placement. Quick movements move it at full speed.
- Flick a ball well past the cushion to take it off the table.

## Dimensions

### Snooker

| Item | Value |
|---|---|
| Playing area | 3,569 × 1,778 mm (11 ft 8½ in × 5 ft 10 in) |
| Ball diameter | 52.5 mm |
| Baulk line | 737 mm from the bottom cushion face |
| D radius | 292 mm, centred on the brown spot |
| Blue | Centre spot |
| Pink | Midway between the centre spot and the top cushion face |
| Black | 324 mm from the top cushion face |
| Reds | 15 in a touching triangle, apex just behind the pink |
| Cushion rubber | 50 mm from nose to rail |
| Corner pockets | 89 mm between the facings at the fall, 97 mm at the nose line, cut along the pocket diagonal |
| Middle pockets | 108 mm at the fall, 122 mm at the nose line, with rounded jaws |

The official rules require pockets to match the WPBSA templates, but the template measurements aren't published. The pocket figures above are typical tournament values.

### English pool (7 ft, blackball rules)

| Item | Value |
|---|---|
| Playing area | 1,829 × 914 mm (72 × 36 in) |
| Object balls | 50.8 mm (2 in) |
| Cue ball | 47.6 mm (1⅞ in) |
| Baulk line | One fifth of the length, 366 mm from the bottom cushion |
| Black spot | Where the diagonals from the corner pockets to the middle pockets cross, 1,372 mm from the bottom cushion |
| Rack | Black on the black spot, in the middle of the third row |
| Cue ball at the break | In baulk. It can be played from anywhere behind the baulk line |
| Pockets | 81 mm between the jaws, which is 1.6 × the ball diameter, with rounded jaws |
| Cushion rubber | 40 mm from nose to rail |

**Check the rack pattern.** The official red and yellow rack order is only published as a diagram, so the pattern used here is a best reading of it:

```
Row 1 (apex, nearest baulk):  Y
Row 2:                        R Y
Row 3:                        R B Y
Row 4:                        R R Y Y
Row 5 (back row):             Y R R Y R
```

Rows are read left to right as seen from the baulk end. To change it, edit the `RACK` constant in `index.html`.

## Adjusting

Every measurement is a constant in the `SNOOKER` and `POOL` definitions near the top of the script in `index.html`. These include the pocket sizes (`corner` and `middle`), the ball radii and the rack.

## Layout code format

A layout code is a prefix followed by a base64url string. The prefix is `s1` for snooker or `p1` for pool. The string holds 3 bytes per ball: a 12-bit x and a 12-bit y, in whole millimetres from the baulk-end, left-hand corner of the playing area. An x of `4095` means the ball is off the table.

The balls are stored in a fixed order:

- **Snooker:** cue ball, yellow, green, brown, blue, pink, black, then reds 1–15.
- **Pool:** cue ball, black, then the reds and yellows in rack order.

## Installing it as an app

Once it's on GitHub Pages, it's a Progressive Web App (PWA). Anyone with the link can install it without an app store, and it gets its own home-screen icon, opens full screen and works offline.

- **iPhone or iPad:** open the link in Safari, tap **Share**, then **Add to Home Screen**. The **Install app** button under More shows these steps.
- **Android:** open the link in Chrome, then tap **Install app** under More, or use the browser menu's **Install app** / **Add to Home screen**. Long-pressing the icon offers shortcuts straight to Snooker or English pool.
- **Windows, Mac or Chromebook:** in Chrome or Edge, click the install icon in the address bar.

Installed copies update themselves the next time they're opened online. After changing `index.html` or the icons, bump `VERSION` in `sw.js` so the old offline copy is replaced.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole app: page, styles and script |
| `manifest.webmanifest` | App name, icons and colours used when it's installed |
| `sw.js` | Service worker that caches the app so it works offline |
| `icons/` | Home-screen, app and browser-tab icons |

## Running it

It's a static site with no build step and no dependencies. The only external request is for Google Fonts, and the page falls back to system fonts without them. To run it, open `index.html` in a browser, or serve the folder with any static server. Installing and offline use only work when it's served over HTTPS, as GitHub Pages does, or from `localhost`.

### Publishing on GitHub Pages

1. Create a new repository on GitHub and upload everything in this folder, including the `icons` folder. You can use **Add file → Upload files**, or push them with git.
2. In the repository, go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**. Select `main` and `/ (root)`, then save.
4. After a minute or so the site is live at `https://<your-username>.github.io/<repo-name>/`. Share links then use that address. Add `#pool` to the end to link straight to the pool table.
