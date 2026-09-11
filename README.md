# Zeroa agent QR generator

A single static page. A Zeroa agent gets a QR card that opens WhatsApp with the message already
written, so the person scanning only has to press send.

Live page: https://zeroa-io.github.io/zeroa-agent-qr/

## Two modes

**Zeroa signups.** The agent pastes their referral link. The QR targets the Zeroa business number
and carries the referral link inside the message, so the scan is recorded against their code.

    https://wa.me/447860015362?text=<url encoded>I would like to know more about Zeroa. <referral link>

**My own WhatsApp.** The agent puts their own number in. The QR targets that number with a short
hello. Nothing is recorded anywhere, it is a contact card.

    https://wa.me/<their number>?text=<url encoded>Hi <first name>, I would like to connect.

The business target is a constant and is never read from the number field. That was the defect in
the version this replaces, where every QR pointed at whatever number the agent typed.

## The parser mirror

`readRef()` and `readLink()` reproduce, line by line, how the referral code is read back out of the
message on the receiving side. That is what lets the page show an agent the code that will actually
be recorded **before** they print anything. It refuses to draw a QR when no code can be read, and
warns when the code was found by position instead of after `ref=`.

**If the receiving parser changes, change these two functions with it.** They are checked against a
separate transcription of it over 109 link shapes plus every referral code currently in use.

## Numbers

`checkNumber()` rejects the local form, the `00` international prefix, a bracketed `(0)` anywhere,
and a trunk zero sitting behind the country code for the four countries the field hint names. It is
deliberately narrow there, because some countries keep a zero after their code and this page gates
nobody by country.

## Building

`index.html` is generated. Do not hand edit it.

The template and the build script live in the Zeroa working folder, not in this repo. Edit the
template, run the build script, then commit and push from here. GitHub Pages redeploys on push
to `main`.

The build refuses to produce a file that has a leftover placeholder, an em or en dash, any non
ascii character in our own text, a control byte, a `font:` shorthand with `inherit` in the family
slot, a text input under 16px, a real person's name as example data, or a business branch that has
stopped targeting the constant.

## Notes

- One file. No build step in the browser and no external requests at runtime, so it works offline.
- The qrcode library is inlined. The Zeroa wordmark is drawn from vector outlines, not an image.
- Nothing an agent types leaves their device. It is kept in `localStorage` under `zeroa-qr-v2`.
- The card sizes its own print width from the QR version, so the module pitch holds however long
  the referral link is.
