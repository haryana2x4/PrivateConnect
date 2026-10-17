# PrivateConnect Web — public deployment starter

## Run locally
1. Install Node.js 18 or later.
2. In this folder, run `npm install`.
3. Run `npm start`.
4. Open `http://localhost:3000`.

## Deploy publicly with Render
1. Create a GitHub repository and upload the contents of this folder.
2. Sign in at https://render.com and choose **New → Blueprint** (or create a Web Service).
3. Connect the repository. Render can use `render.yaml`; the equivalent settings are:
   - Build command: `npm install`
   - Start command: `npm start`
   - Health check path: `/health`
4. Wait for deployment and use the HTTPS URL Render gives you. Anyone with that URL can access the site from any network, subject to the host's availability and service limits.

The server uses `process.env.PORT` and binds to `0.0.0.0`, as required by common hosting platforms. HTTPS is provided by the hosting platform; do not share an HTTP URL for private use.

## Encryption and important limitations
- The browser encrypts message/photo payloads with AES-GCM before sending them to the server. The server's chat relay does not decrypt those payloads.
- This is a prototype, NOT an audited secure messenger. Everyone in a room shares the same phrase; there is no forward secrecy, verified identity/key fingerprints, or safe key rotation. Anyone who learns the phrase may decrypt captured messages encrypted with it.
- In a web app, the hosting server serves the JavaScript that performs encryption. A malicious or compromised deployment could serve modified JavaScript that steals the phrase or plaintext. HTTPS protects transport but does not by itself solve this trust problem.
- Room names, membership, IP/connection metadata, and timing remain visible to the server. Rooms are held in memory and disappear when the server restarts. No database, durable history, rate limiting, or production hardening is included.
- Do not use this prototype for highly sensitive conversations until the cryptographic design and deployment have been reviewed by a security professional.
