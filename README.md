# P2P Tetris

Two-player Tetris that runs entirely in the browser. Static files only, so it hosts for free on GitHub Pages. Players connect directly over WebRTC using [PeerJS](https://peerjs.com/)'s free public signaling server.

## Deploy
1. Create a GitHub repo and push these files to the `main` branch.
2. Open **Settings → Pages**, set **Source** to "Deploy from a branch", choose `main` and `/ (root)`, then save.
3. Your game is live at `https://<user>.github.io/<repo>/` within a minute or two.

## Play
- **Create a room**, then send the link to a friend. They open it and connect automatically.
- The host starts each round. Cleared lines send garbage to the opponent (2 lines = 1, 3 = 2, 4 = 4). Clearing lines also cancels garbage you have incoming (the red bar).
- Controls: arrows move, Up/X rotate, Z rotate back, Space hard drop, C hold.

## Run locally
ES modules need a web server: `python3 -m http.server`, then open http://localhost:8000.

## How it works
- `js/tetris.js` is the game engine with no DOM or network code. Both players use the same seed, so piece sequences match.
- `js/net.js` wraps PeerJS. The host registers the ID `p2p-tetris-<code>` and the guest connects to it.
- `js/main.js` handles UI, input, rendering and the messages (`start`, `atk`, `board`, `dead`).

## Notes
- Some strict networks (corporate, some mobile carriers) block direct WebRTC and need a TURN relay. Add one to `iceServers` in `js/net.js`.
- If the public PeerJS server is down, new connections fail but games in progress continue. You can self-host `peerjs-server` and pass `host`/`port` in `OPTS`.
- Pin or vendor the PeerJS script if you want it independent of the CDN.
