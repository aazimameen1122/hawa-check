# Hawa Check

Kashmir Valley ki hawa, saada alfaaz mein — one phone-friendly page that shows the
current air quality and the next 48 hours for five Valley towns:
**Srinagar, Pampore, Anantnag, Baramulla, Sopore.**

## What it does

- Fetches the **Open-Meteo Air Quality API live in the browser** every time the page
  opens (no baked-in snapshot, no server, no API key).
- Shows per town: current **US AQI** with band colour, **PM2.5**, **PM10**,
  European AQI, plain-language guidance for the band, and the model-data timestamp.
- Tap any town card for its **next-48-hours hourly AQI strip**.
- If the API can't be reached, the card says so — it never shows a stale or
  invented number.

## Data source and its limits — read this first

All numbers are **forecasts from CAMS atmospheric models via Open-Meteo.
Not a ground sensor reading.** CAMS is a global atmospheric model on a coarse
grid (the API resolved our towns to ~0.1° model points), so:

- It can disagree with a real ground sensor in the same town, especially in a
  valley under a temperature inversion.
- Nearby towns can fall in the same model cell and return identical numbers —
  at build time, **Srinagar and Pampore returned byte-identical series**, and
  their cards say so on the page. Identical numbers here are one model value,
  not two independent readings.
- US AQI is the maximum across several pollutants (PM2.5, PM10, ozone, …),
  which is why the AQI can look high while PM2.5 alone looks low.

Guidance text is fixed per standard US AQI health band. It is band guidance,
not medical advice.

## Run it

It's one static file: `index.html`. Open it in any browser, or serve the folder:

```bash
python3 -m http.server 8000   # then open http://localhost:8000
```

(Opening the file directly also works in most browsers; serving is safer for
mobile testing on the same Wi-Fi.)

## Deploy (one step, needs repo owner's go-ahead)

1. Push this repo to GitHub.
2. Repo → Settings → Pages → deploy from branch `main`, folder `/ (root)`.
3. The page is live at `https://<user>.github.io/<repo>/` — no build step.

## Files

- `index.html` — the app (single file, live fetch, no libraries).
- `build.py` — **snapshot utility from the micro version**: regenerates a static
  `snapshot.html` from local `srinagar.json` / `pampore.json`. Not used by the app.
- `RESULTS.md` — build + verification record, test-time values, limitations.
