# Airplane Chat

A tiny browser-only 1:1 chat for two nearby Android phones on the same Wi-Fi hotspot.

## Important limitations

- Both phones must open this HTTPS site once while internet is available so the PWA files can be cached.
- For offline use, one phone enables its Wi-Fi hotspot and the other joins it. Mobile data/internet can be off.
- Pairing is a two-step manual WebRTC exchange: creator shares an offer code; joiner returns an answer code.
- This version uses no STUN/TURN servers and tries direct local-network connectivity. Some Android hotspot implementations isolate connected clients or Chrome may not expose usable local candidates. If so, the connection will fail; this browser-only design cannot override Android hotspot restrictions.
- Keep the browser tab open and active. It does not receive messages in the background.
- Text messages are stored locally in each browser. This MVP does not synchronize old history or provide group chat/file transfer.
- WebRTC transport is encrypted with DTLS. The connection codes are sensitive temporary connection data; share them only with the intended friend.

## Deploy to GitHub Pages

1. Create a GitHub repository named `airplane-chat`.
2. Upload `index.html`, `manifest.json`, and `sw.js` to the repository root.
3. Open **Settings → Pages**.
4. Under Build and deployment, choose **Deploy from a branch**, select `main` and `/ (root)`, then Save.
5. Wait for GitHub Pages to publish the HTTPS URL.
6. Open that URL in Chrome on both phones while internet is available. Keep the page open briefly so the service worker can cache it.
7. Test once while still online, then test offline.

## How to connect offline

1. Phone A turns on its Wi-Fi hotspot.
2. Phone B joins Phone A's hotspot. Either phone may be the hotspot host.
3. Both open the already-cached chat page in Chrome.
4. Phone A taps **Create connection code**, copies it and sends it to Phone B using any offline method (nearby screen, text file, etc.).
5. Phone B pastes the code under **Join chat** and taps **Create reply code**.
6. Phone B sends the reply code back to Phone A.
7. Phone A pastes it under **Paste your friend's reply code** and taps **Connect**.
8. When the status says Connected, chat.

## Why two codes?

A shared Wi-Fi password or short PIN is not enough for browser-to-browser WebRTC. Without a signaling server, the browsers must exchange WebRTC offer/answer details manually. This app packages those details into copyable codes.

## Test checklist

- Test with both phones on the same hotspot.
- Keep both Chrome tabs open and screens awake.
- If it fails, try reversing which phone hosts the hotspot.
- Some devices block client-to-client traffic on hotspots. This cannot be fixed by this web app alone.
