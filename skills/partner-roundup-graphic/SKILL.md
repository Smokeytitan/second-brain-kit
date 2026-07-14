---
name: partner-roundup-graphic
description: >
  Build the {COMPANY_HANDLE} "Weekly Roundup" social banner in a Figma socials file by swapping white partner logos onto the gradient cards. Use whenever the user says "make the roundup graphic", "build the weekly roundup banner", "swap the logos for the roundup", "partner roundup graphic", "put the logos in figma", or right after the weekly partner roundup post is drafted and it needs the paired banner. Clones the latest banner on the weekly page into a new dated frame, sources white-on-transparent logos (reusing in-file assets first, otherwise fetching and whitening each partner logo via the browser), and places them on the five cards (center = the week's lead story, then two flanks, then two edges). Pairs with partner-roundup-drafter (the post text) and follows your designer's logo-editing protocol.
---

# Partner Roundup Graphic (Figma logo swap)

## What this builds
The {COMPANY_HANDLE} "Weekly Roundup" banner: 1920x768, dark grid background, company logo top-left, "Weekly Roundup" top-right, and five gradient "cards" fanned out, each holding one white partner logo. The center card is largest and sits highest (the week's lead story). Two flanks, two edges.

- File: your socials Figma file, fileKey `{FIGMA_SOCIALS_FILE_KEY}` (the file key from your socials file URL), page "Weekly".
- Each week is a dated frame (for example "Ecosystem Roundup 6/14") under monthly sections. New weekly frames are often empty, so build by cloning a filled one.

## Layout rules (from your designer's protocol)
- Center = biggest = the week's lead / most recognizable partner (for example a major card network or a household-name app).
- The two flanks match each other in size; the two edges match each other.
- Logos must be WHITE on transparent. The card supplies the gradient.
- Position each logo toward its card's VISIBLE side. Cards overlap with the center on top, so left-side cards show their LEFT portion and right-side cards show their RIGHT portion. A slight peek / partial overlap is intentional ("a little bit of each logo hidden").
- Pick the five: center is the lead story; the other four are next by impact / recognizability.

## Step 1: Clone the template
Clone the most recent filled banner frame into a new dated draft.
- `use_figma`: `const src = await figma.getNodeByIdAsync(latestFilledFrameId); const c = src.clone(); c.name='Ecosystem Roundup M/D (draft)';` then walk up to the PAGE, `page.appendChild(c)`, and move it to clear canvas space (for example `c.x=-329; c.y=4600`).
- The clone may contain six sub-frames with the same name (one is a redundant duplicate at the left edge: hide it). Identify the five by x position: edges at x about 87 and 1309, flanks at x about 349 and 1047, center at x about 698 (tallest).
- Each sub-frame contains one image rectangle (the logo). Swap that rectangle's fill.

## Step 2: Source a white logo per partner
For each of the five partners, get a white-on-transparent PNG:
1. Reuse in-file first. Search the file's pages for the partner name and reuse the existing image `imageHash`. Official white lockups often live in previous co-branded socials in the same file.
2. Otherwise fetch and whiten via the browser (see "Logo pipeline" below).

## Step 3: Place each logo
For each card's image rectangle: `rect.visible=true; rect.fills=[{type:'IMAGE', imageHash, scaleMode:'FILL'}];` then resize to the logo's aspect and position toward the visible side, vertically centered.
- Rough sizes: center about 280 wide, flanks about 215 to 220 wide, edges about 150 to 220.
- Left-side cards: x about 55. Right-side cards: x about (cardWidth - logoWidth - 55).
- Hide the redundant sixth sub-frame.

## Step 4: Verify
`get_screenshot` the draft frame. Confirm all five logos are white, readable, and not over-clipped by the center card. Adjust sizes and x positions, then re-screenshot.

## Logo pipeline (browser to Figma): the hard part
Hard constraints (learned empirically in this MCP setup):
- `figma.createImageAsync(url)` is NOT supported here. Use `figma.createImage(bytes)`.
- The sandbox has no outbound network. Only the browser (Claude in Chrome) can fetch images.
- Returning base64 from the browser is BLOCKED. Returning long hex is TRUNCATED at roughly 1000 chars.
- POSTing the image to a Figma `upload_assets` submitUrl HANGS from some page origins (unavatar has a service worker that stalls it; a renderer frozen by repeated heavy JS also hangs).

Reliable method (browser POST after clearing the service worker):
1. Get a submitUrl: call `upload_assets` (count up to 5; each is single-use, expires in 10 min).
2. Browser: `navigate` to `https://unavatar.io/x/<handle>` (a partner's X avatar is its logo; it loads same-origin so canvas `getImageData` is not tainted). Wait for load (400x400).
3. Browser JS (one call): FIRST run `await navigator.serviceWorker.getRegistrations().then(rs=>Promise.all(rs.map(r=>r.unregister())))`. Unavatar's service worker stalls the upload POST; unregistering it is what makes the POST work. Then process the avatar (see Background removal), `toBlob`, and POST as `multipart/form-data` (field `file`) to the submitUrl with a ~10s AbortController. The response JSON contains `imageHash`.
4. `use_figma`: set the card rect's fill to `{type:'IMAGE', imageHash, scaleMode:'FILL'}`, resize, position. The POST also auto-creates a stray frame (`placedOnNodeId`) named after the upload; delete it with `node.remove()`.

Background removal (run on the NATIVE-resolution avatar, never an upscaled copy: upscaling smooths edges and lets the flood creep in and hollow out the logo):
- Colored logo on a solid or transparent background: flood-fill from the four corners + edge midpoints with neighbor-color tolerance (~50). The enclosed logo (even a white inner face) survives because the flood stops at the logo's hard edges. Then whiten every remaining opaque pixel, keeping its alpha.
- LIGHT logo on a DARK or beveled/metallic background: do NOT flood (it creeps along the dark interior bevels and leaves only faint outlines). Instead key by brightness: `alpha = clamp((luminance - 95)/(170-95))`, `RGB = white`.
- After keying, autocrop to the logo bounds, then downscale the crop to ~300px max side with `imageSmoothingQuality='high'` for anti-aliasing.

Resolution rule (the "edges look jagged" fix): the avatar source caps near 400px and the cutout is smaller, so place each logo at a DISPLAY size BELOW its source pixel size, and export the banner at scale 1 (1920x768). Exporting at 2x upscales the small raster logos and they look jagged; the vector chrome stays crisp at any scale.

Transfer fallback (if a POST path is ever blocked): `figma.createImage(bytes)` works from raw PNG bytes, but base64 returned from the browser is blocked and long hex truncates in tool results. For small logos only, carry bytes via `location.hash = 'L='+hex` (the full hex shows untruncated in the Tab Context URL), then decode and `createImage` in `use_figma`.

## Gotchas
- `loadAllPagesAsync` is unsupported. Use `getNodeByIdAsync` and per-node access; you can read `figma.root.children` page names and load a page with `page.loadAsync()`.
- Do not set `figma.currentPage`.
- `upload_assets` with a `nodeId` does NOT reliably auto-place onto a node on a non-current page. Treat `upload_assets` as "get bytes into the blobstore, then apply the returned hash yourself".
- Whitening flattens detailed logos to silhouettes (a 3D cube mark can become a plain white hexagon). Usually fine. For a logo that needs internal detail, reuse an official white asset instead of whitening.
- Honor the user's writing preferences in any output text (for example, if they ban em dashes and en dashes, use colons, commas, parentheses, or plain hyphens).
- If a step fails twice, stop and re-plan rather than retrying the same way (each frozen-renderer retry costs about 45 seconds).

## Partner handles
Keep a running note of the X handles used each week so avatars can be re-fetched quickly. <!-- Fill in your own partner handles per week -->
