# SonoFlow — hosted pages

Published with GitHub Pages at `https://Khoshkhah.github.io/sonoflow-presentation/`.

| page | what it is |
|---|---|
| `index.html` | the documentation hub — the whole manual, every diagram, the model term by term |
| `presentation.html` | the pilot overview and pitch deck |
| `viz/app.html` | the Studio: the map, one date, every street and hour |
| `viz/sensors.html` | the sensor view |

The hub links to the other two, so it is the place to start.

## The passphrase

`index.html` and `presentation.html` ship **encrypted**, not gated. The file contains ciphertext;
the browser derives a key from what the reader types and decrypts it there — AES-256-GCM with
PBKDF2-HMAC-SHA256, 300,000 iterations. There is no plaintext to read around, and a wrong
passphrase fails to authenticate rather than rendering anything.

Ask the owner for it. It is not written here, and it should not be committed to this repository:
history is public, so a passphrase that lands in a commit stays published even after the line is
deleted.

One passphrase serves everyone, and revoking it means re-encrypting and re-publishing. That is the
right trade for material that is *rather not public*, and the wrong one for material that must not
leak — that needs a real access proxy in front of a private host.

`viz/` is deliberately not encrypted: the Studio is 35 MB, and encrypting it would push the file
past GitHub's size warning while forcing a second unlock to open it from the hub.

## Rebuilding

From the SonoFlow repo, beside this one:

```bash
python3 docs/build_hub.py                                             # -> docs/hub.html
python3 docs/encrypt_hub.py --out ../sonoflow-presentation/index.html
PYTHONPATH=<roadstyle>/src python3 -m viz.app --date <YYYY-MM-DD>     # -> viz/app.html
cp viz/app.html ../sonoflow-presentation/viz/app.html
```

`docs/encrypt_hub.py` prompts for the passphrase, or reads `HUB_PASSPHRASE`.
