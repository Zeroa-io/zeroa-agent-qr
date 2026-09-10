# Zeroa agent QR generator

A single static page. A Zeroa agent pastes their referral link and gets a QR card that opens
WhatsApp to the Zeroa business number with the message already written.

Live page: https://zeroa-io.github.io/zeroa-agent-qr/

## What the QR encodes

    https://wa.me/447860015362?text=<url encoded>Zeroa signup please. <the agent's referral link>

The whole referral link rides inside the message, so the referral code travels with every scan.

## The parser mirror

`readRef()` and `readLink()` in the page reproduce, line by line, how the referral code is read
back out of that message on the receiving side. That is what lets the page show an agent the code
that will actually be recorded **before** they print anything. It refuses to draw a QR when no code
can be read, and warns when the code was found by position instead of after `ref=`.

**If the receiving parser changes, change these two functions with it.** They are checked against a
separate transcription of it over 109 link shapes plus every referral code currently in use.

## Building

`index.html` is generated. Do not hand edit it.

The template and the build script live in the Zeroa working folder, not in this repo. Edit the
template, run the build script, then commit and push from here. GitHub Pages redeploys on push
to `main`.

The build refuses to produce a file that has a leftover placeholder, an em or en dash, any non
ascii character in our own text, a control byte, a `font:` shorthand with `inherit` in the family
slot, a text input under 16px, or a surviving reference to any of the fields removed from the old
version.

## Notes

- One file. No build step in the browser and no external requests at runtime, so it works offline.
- The qrcode library is inlined. The Zeroa wordmark is drawn from vector outlines, not an image.
- Nothing an agent types leaves their device. It is kept in `localStorage` under `zeroa-qr-v2`.
