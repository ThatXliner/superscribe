# Superscribe

This is an AI app that diagrams what you say in real time. I should've open sourced this a while ago, but I was too busy working on other projects and I was originally thinking of making a business out of it.

Currently, it does not have:

- A decent UI
- A hosted demo
- Live transcription



Here's a very old demo of the app (and it's pretty bad, especially since Superscribe is intended to long-form content):

https://youtu.be/jFX70norE-4

## TLS runtime requirements

Build and install with Node.js `^22.23.4 || ^24.22.1 || >=26.12.1`. The `.nvmrc` pins 22.23.4. Older Node releases are unsupported following the [eMudhra intermediate CA compromise](https://emudhra.we-have-a-problem.com/). Use the committed lockfile with `pnpm install --frozen-lockfile`. Bun is not a supported installer while its certificate fix is pending.

This floor covers JavaScript build tools and package downloads. Browser and Tauri webview HTTPS requests use the browser or operating system verifier, so keep those updated too.

Run `node scripts/check-tls.mjs` to verify both compromised test chains are rejected and `https://example.com/` still succeeds. Timeouts and DNS errors fail this check.
