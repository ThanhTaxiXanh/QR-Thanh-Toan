# QR Thanh Toán — Device controller + Device display

1. Create a Firebase project (console.firebase.google.com) and add a Web app.
2. Build > Realtime Database > Create database.
3. Build > Authentication > Sign-in method > enable **Anonymous** (devices get a private uid; the rules use it).
4. Copy the web config into `firebaseConfig` near the end of `index.html` (search `YOUR_API_KEY`).
5. Realtime Database > Rules: paste `database.rules.json` and publish.
6. Run locally: `python3 -m http.server 8080` (or any static server). Wake Lock, Fullscreen and share need HTTPS (localhost is fine).
7. Deploy `index.html` to Firebase Hosting / Cloudflare Pages / Netlify / GitHub Pages. Add the domain under Authentication > Settings > Authorized domains.
8. Android: open the site > "Android / Màn hình hiển thị" > "Tạo phiên kết nối". iPhone: scan the pairing QR (or type the 16-character code).

Notes
- Only structured payment fields are synced; each device builds the VietQR payload and QR locally with the same code.
- Pairing secret: the 16-character code = 8-char session ID + 8-char join token; the rules only let a device that knows the token become controller.
- Sessions expire after 30 min without any heartbeat (`SESSION_TTL_MS`); expired sessions are unreadable and the display deletes them. Orphans need a scheduled cleanup (e.g. Cloud Function) if you want them physically removed.
- Production: self-host `qrcode.min.js` and the three Firebase compat scripts instead of the cdnjs/gstatic URLs.
