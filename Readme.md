ZLMRTCClient.js
------
ZLMRTCClient.js is javscript webrtc client sdk for  ZLMediakit 

how to build:
-------
```bash
npm install
npm run build # or npm run pro
```

Changelog
-------

### 2026-01-02: Fix race condition in setRemoteDescription

**Problem:** `TypeError: Cannot read properties of null (reading 'setRemoteDescription')` occurred when the PeerConnection was closed during the HTTP offer/answer exchange.

**Root cause:** When user quickly closes the video player while the axios HTTP request is still pending, the `close()` method sets `this.pc = null`. When the HTTP response arrives, the code tries to call `this.pc.setRemoteDescription()` on a null object.

**Fix:** Added defensive checks before calling `setRemoteDescription()`:
- Null check: `if (!this.pc || this.pc.connectionState === 'closed')`
- Try-catch block to catch any remaining race condition errors

**Files changed:** `src/endpoint/endpoint.js`