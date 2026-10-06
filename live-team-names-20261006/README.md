# Hackathon Team Name Finder

This local Coworker suggests five team names and recommends one.

## Run

1. Install Node.js 24 or newer and Sokosumi CLI 1.0.4.
2. Run `sfw npm ci` in this directory.
3. Copy `.env.example` to `.env`, set permission 600, and enter the model settings and Coworker ID.
4. Run `npm start`, then `npm run worker` in a second terminal.

Keep `EVE_PORT` and `EVE_URL` consistent. The server binds to `127.0.0.1`.
Import the Coworker runtime key with the CLI. Keep it in the OS vault.
The worker uses the supported CLI credential interface. It does not use your account token for runtime writes.

## Payments

Set `PAID_TASKS_ENABLED=true` only after funding and confirmed registration.
The default quote is 1 test USDM, or `1000000` atomic units, on Cardano Preprod.
Use a dedicated payment database and seller wallet. Keep generated configuration in `.local/mps.env`.
Keep the scoped ReadAndPay token in `.local/mps-runtime.env`. Run `npm run api` for the agent API.
`payment-registration.mjs` uses the local admin key only for setup. The worker uses the scoped key for payments.

The worker waits for confirmed escrow before the model turn. It saves the exact result before submission.
Unknown writes stay pending. Inspect their saved state before recovery.
Paid result hashes remain unchanged when the Coworker replies to later human comments.

The Standard API uses nonce-prefixed hashes from MIP-004. Direct Task payments use raw UTF-8 SHA-256 hashes.
Settlement verification matches Core and MPS transaction hashes, then measures the seller's net token receipt through Blockfrost.
Task completion and workspace credit charges do not prove seller receipt.

## Checks and evidence

Run `npm test` for the local checks. Tests do not prove live settlement.
Session evidence is in `docs/`. The supplied CSV stays unchanged in `data/`.
Services are local. This machine must stay awake.
