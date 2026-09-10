# Zeroa agent QR generator

A single static page. An agent pastes their Zeroa referral link and gets a QR card that opens
WhatsApp to the Zeroa business number with the message already written.

Live page: https://booysenmarkus.github.io/zeroa-agent-qr/

## What the QR encodes

    https://wa.me/447860015362?text=<url encoded>Zeroa signup please. <the agent's referral link>

`Zeroa_WhatsApp_Sales_Inbox` in Zoho CRM reads the referral code out of that message, creates or
matches the Lead, replies on WhatsApp and runs the enrichment sequence. The whole referral link
rides in the message and is echoed back to the prospect verbatim.

## The parser mirror

`readRef()` and `readLink()` in the page are a line by line mirror of section 4 of that Deluge
function, so the page can tell an agent the code the CRM will actually read **before** they print
anything. It refuses to draw a QR when no code can be read, and warns when the code was found by
position instead of after `ref=`.

**If the Deluge function's parser changes, change these with it.** They are checked against a
Python transcription of the Deluge over 109 link shapes plus every live referral code in the CRM.

## Building

`index.html` is generated. Do not hand edit it.

Source and build script live in the project working folder, not in this repo:

    claude knowledge base docs\WhatsApp QR Lead Capture\_app_template.html
    claude knowledge base docs\WhatsApp QR Lead Capture\_build_app.py

Edit the template, run `python _build_app.py`, then commit and push from this folder. GitHub Pages
redeploys on push to `main`.

The build fails on a leftover placeholder, an em or en dash, any non ascii in our own text, a
control byte, a `font:` shorthand with `inherit` in the family slot, a text input under 16px, a
surviving reference to the deleted number field or the old SMS mode, and Ken's live agent code.

## Notes

- One file, no build step in the browser, no external requests at runtime.
- The qrcode library is inlined. The Zeroa wordmark is drawn from vector outlines, not an image.
- Nothing an agent types leaves their device. It is kept in `localStorage` under `zeroa-qr-v2`.
