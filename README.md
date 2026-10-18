# MTIS Natural Mineral Water — site concept

A single page site for MTIS natural mineral water (Natakhtari, Mtskheta, Georgia).
Dark cinematic hero with a waterline, a source cross-section, the full product range,
certificates, the Paris Saint-Germain partnership and the hospitality partners.

## Running it

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8777
```

## What is in here

| path | |
|---|---|
| `index.html` | the whole site: markup, styles and scripts in one file |
| `assets/products/` | the seven bottle renders |
| `assets/certificates/` | SGS certificates (ISO 9001:2015, FSSC 22000) |
| `assets/partners/` | PSG campaign images and the hospitality logos |
| `assets/scene/` | hero glacier, water reflection, cross-section imagery |
| `assets/fonts/` | FiraGO subset (Latin + Georgian, carries the mtavruli `case` feature) |
| `assets/docs/` | press releases |

## Notes

- Two languages, EN and KA, switched in the header and remembered per browser. English is the default.
- Georgian headings are set in mtavruli through FiraGO's OpenType `case` feature, body text stays mkhedruli.
- No build step and no framework. Display type is Instrument Serif, UI type is Inter.
- Copy and imagery come from mtiswater.com. The brand assets belong to MTIS / Coca-Cola Bottlers Georgia,
  the PSG material to Paris Saint-Germain, and the partner logos to their owners.
