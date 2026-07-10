# excise

**Remove C2PA provenance manifests, EXIF, and XMP metadata from images — entirely in your browser.**

excise is a single-file, dependency-free tool for stripping [C2PA / Content Credentials](https://c2pa.org/) and other embedded metadata out of images. There is no backend: every byte is processed locally in your browser, so **your images are never uploaded, logged, or sent anywhere.**

[**▶ Live demo**](https://your-username.github.io/excise/) · MIT licensed

---

## Features

- **Lossless for JPEG & PNG** — excise edits the file structure directly and removes only the metadata segments. Pixel data is never re-compressed, so there is zero quality loss.
- **Removes C2PA manifests** — targets `APP11 / JUMBF` segments in JPEG and `caBX` chunks in PNG (the containers C2PA uses).
- **Optional EXIF & XMP removal** — strip location, device, timestamp, and edit-history data before sharing.
- **Detection first** — every file is inspected on load, so you can see exactly what metadata is present (and its byte size) before removing anything.
- **Drag & drop or click to upload** — handles multiple files at once.
- **100% client-side** — no server, no tracking, no dependencies. Save the page and it works offline.
- **Fallback for other formats** — WebP and others are handled via a canvas re-encode (note: this recompresses the image).

---

## How it works

| Format | Method | Quality |
|--------|--------|---------|
| JPEG | Removes `APP11` (JUMBF/C2PA), optionally `APP1` (EXIF/XMP) segments | Lossless |
| PNG | Removes `caBX` (C2PA), `eXIf`, and C2PA/XMP text chunks | Lossless |
| WebP / other | Canvas re-encode (strips all metadata) | Recompressed |

For JPEG and PNG, excise walks the raw byte stream, identifies the marker segments / chunks that hold metadata, and rebuilds the file without them. Kept chunks are copied verbatim, so PNG CRCs stay valid and JPEG scan data (including `FF00` byte-stuffing) is preserved exactly.

---

## Run locally

No build step. Just open the file:

```bash
git clone https://github.com/sshnox/excise.git
cd excise
open index.html      # or double-click it
```

Or serve it:

```bash
python3 -m http.server 8000
# visit http://localhost:8000
```

---

## Deploy to GitHub Pages

1. Push this repo to GitHub.
2. Edit the `REPO_URL` constant near the top of the `<script>` in `index.html` so the **Source** link points at your repo.
3. Go to **Settings → Pages**.
4. Under **Build and deployment**, set **Source** to *Deploy from a branch*, pick your `main` branch and the `/ (root)` folder, and save.
5. Your tool goes live at `https://your-username.github.io/excise/` within a minute or two.

Because everything is a single static `index.html`, no Actions workflow or bundler is required.

---

## A note on responsible use

C2PA / Content Credentials exist to make image provenance transparent — including whether AI was involved in creating or editing an image. Stripping that data has plenty of legitimate uses: removing your GPS location, camera serial number, or editing history before posting a photo publicly, reducing file size, or fixing tools that choke on unexpected metadata.

C2PA is designed to be *tamper-evident*: removing a manifest doesn't forge a valid one — it simply returns the image to the same "unknown provenance" state as the vast majority of images online. Please use excise thoughtfully, and respect content authenticity in contexts where it matters.

---

## License

MIT — see [LICENSE](LICENSE). Contributions welcome.
