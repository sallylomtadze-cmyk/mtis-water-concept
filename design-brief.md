# MTIS site — brief for an AI model (DeepSeek / GPT / Claude)

## როგორ გამოიყენო

1. გახსენი DeepSeek-ის ახალი ჩატი.
2. ჩააკოპირე ქვემოთ მოცემული **PROMPT** სრულად.
3. **აუცილებლად** მის შემდეგ ჩააკოპირე ჩვენი `index.html`-ის სრული კოდი (80KB, ~1000 ხაზი — DeepSeek-ის კონტექსტში ეტევა). ამის გარეშე მოდელი ნულიდან დაიწყებს და ჩვენს ნამუშევარს არ გააგრძელებს. თუ ვერ ეტევა, ჩააკოპირე მხოლოდ ის სექცია + მთლიანი `<style>` ბლოკი.
4. ბოლოს დაამატე ერთი წინადადება: `Now rework only the <section id="…"> block. Return a unified diff.`
5. თუ სკრინშოტის ატვირთვა შეგიძლია — ატვირთე. ტექსტით აღწერა ვერასდროს ჩაანაცვლებს იმას, რასაც მოდელი თვალით დაინახავს.
6. **ერთ ჯერზე ერთი სექცია.** „მთელი საიტი გააუმჯობესე" ყოველთვის ზოგად და უარეს შედეგს იძლევა, ვიდრე „ეს ერთი ბლოკი ასე გადააკეთე".
7. პასუხი რომ მოვა, არ ჩააკოპირო პირდაპირ ფაილში — მომეცი და მე შევამოწმებ, რომ `data-ka`, i18n და ანიმაციები არ გატყდეს.

---

## PROMPT (ჩასაკოპირებელი)

You are a senior web designer and front-end engineer. You are improving a live single-page site
for MTIS, a Georgian natural mineral water brand. Work at the level of an award-winning studio:
restrained, editorial, premium. No template look, no stock gradients, no emoji.

### Continue the existing site — do not restart it

The site is already built and live. I am pasting its real code below. You are editing that file,
not designing a new one. Specifically:

- Never return a fresh page from scratch, a new structure, or placeholder content. If you cannot
  see a part of the code you need, ask for it instead of inventing a replacement.
- Keep every existing class name, id, CSS custom property and JS hook unless the change is
  impossible without renaming, and say so when you rename one.
- Build new pieces out of the system already in the file (listed under "The system you must build
  inside"). A new component that introduces its own colours, spacing or animation vocabulary is a
  wrong answer even if it looks good on its own.
- Preserve everything in the section you touch that I did not ask you to change: copy, links,
  image paths, translations, the reveal behaviour, the numbering.

### The project

- One file: `index.html` (~1000 lines). Markup, CSS and JS are all inside it. No build step,
  no framework, no bundler. It is served as a static file from GitHub Pages.
- Local assets live in `assets/` (54 files, 8 MB): real product renders, certificates, partner
  logos, landscape photography, a FiraGO webfont subset.
- The only external dependencies are Google Fonts (Inter, Noto Sans Georgian) and three.js
  0.160.0 from jsDelivr, used for a real-time 3D bottle in the hero.
- Bilingual, EN and KA. Every translatable element carries a `data-ka="…"` attribute and a tiny
  script swaps `innerHTML` between the cached English markup and the Georgian one. English is
  the default; the choice is stored in localStorage.
- Georgian headings render in mtavruli through FiraGO's OpenType `case` feature
  (`font-feature-settings:"case" 1` under `html[lang=ka]`), not through separate characters.

### Hard constraints — breaking any of these makes the answer useless

1. Keep every `data-ka` attribute intact. If you add visible text, add its Georgian translation
   in a `data-ka` attribute on the same element.
2. Do not add dependencies. three.js may be used, nothing else. No Tailwind, no GSAP, no React.
3. Do not invent facts, numbers, dates, awards or claims about the brand. Use only the data
   listed below. If a section needs more copy, rephrase what exists.
4. Keep it one file. Keep asset paths exactly as they are.
5. Responsive from 375px to 1920px, no horizontal scroll at any width.
6. Respect `prefers-reduced-motion`. Animation must stay above 50fps on a 2019 laptop; pause
   anything offscreen with IntersectionObserver.
7. Keep the light/dark section rhythm (dark hero → dark about → light source → dark products →
   light quality → dark partners → dark finewaters → footer).

### The system you must build inside

Tokens, declared once on `:root` and overridden per band. Use these, never raw hex:

```css
:root{
  --ink:#04080e; --deep:#07141f; --panel:#091a27;
  --accent:#6fd8ec; --accent-2:#2a9cb8;
  --text:#e3f2f8; --muted:#8aa8b8; --line:rgba(140,200,225,.16);
  --glass:linear-gradient(170deg,rgba(255,255,255,.085),rgba(255,255,255,.02));
  --gut:28px;      /* page gutter, 16px under 700px */
  --r:22px;        /* the standard corner */
}
.band.light{ --text:#0a2634; --muted:#5a7b8b; --line:rgba(10,45,62,.15); --accent:#0d6f88;
             --glass:linear-gradient(170deg,#fff,rgba(255,255,255,.66)); }
```

Patterns already in use — extend them, do not invent parallel ones:

- `.band` is a section; `.band.deep` is the darker variant, `.band.light` flips the tokens and
  rounds its top corners while pulling up into the previous band.
- `.wrap` centres content: `width:min(1240px,100%)`, `padding-inline:var(--gut)`.
- `.rev` is the reveal: starts at `opacity:0; translateY(42px); blur(6px)`, gets `.in` from a
  single IntersectionObserver. `.d1`–`.d4` stagger it. Any new block should carry `.rev`.
- `.tag` is the section eyebrow; `data-n="03"` prints the section number through `::before`.
- `h1, h2` are FiraGO 600 uppercase; Georgian gets mtavruli from the `case` feature. Body is
  Inter 300. Labels are Inter, 10–11px, uppercase, letter-spacing .2em.
- Cards are `background:var(--glass)`, `1px solid var(--line)`, `backdrop-filter:blur(14px)`.
- Buttons are `.btn`: pill, hairline border, uppercase 10.5px, .22em tracking.
- The hero exposes `--wl` (the waterline, 36%; 33% on mobile) — anything that must align with the
  water reads from it.
- Easing is `cubic-bezier(.2,.8,.3,1)`; durations .45–.95s. Nothing bounces.

### The brand data (this is all of it — invent nothing)

- MTIS natural mineral water, sourced in Natakhtari, Mtskheta municipality, Georgia.
- Bottled by "Coca-Cola Bottlers Georgia" JSC, certified by SGS United Kingdom Ltd.
- Certifications: ISO 9001:2015 (quality management), FSSC 22000 (food safety).
  HACCP and FSMS systems aligned with Coca-Cola KORE protocols. Annual quality testing in Geneva.
  Products meet the criteria for export to the European Union.
- Bromate: MTIS is the Georgian brand with the least bromate in it, close to 0%.
- Seven formats, with case and pallet data:
  | format | per case | per pallet |
  |---|---|---|
  | 0.330 GLS | 15 bottles | 132 P/C |
  | 0.500 GLS | 12 bottles | 102 P/C |
  | 0.500 PET | 12 bottles | 144 P/C |
  | 0.750 GLS | 6 bottles | 150 P/C |
  | 1.500 PET | 6 bottles | 112 P/C |
  | 3.000 PET | 4 bottles | 77 P/C |
  | 5.000 PET | 2 bottles | 84 P/C |
- Official partner of Paris Saint-Germain, the only Georgian brand to join so far.
- Member of FineWaters.com, founded by Dr. Michael Mascha in 2002.
- Hospitality partners: Ambassadori, The Biltmore, Pullman, Radisson RED, Hilton Garden Inn,
  Adjara Group, M Group, Oro & Strada.
- Tagline in use: "Spend it all. Refill." / "დახარჯე სულ. შეავსე."

### The assets you can place

```
assets/products/0330-gls.png … 5000-pet.png   seven bottle renders, 609×960, transparent
assets/certificates/cert-1.png, cert-2.png    SGS certificates, 1319×1867
assets/partners/psg-1..3.png                  PSG campaign photography, 1200×650
assets/partners/psg-lockup.png                MTIS × PSG lockup
assets/partners/*.png                         eight hospitality logos + finewaters
assets/scene/landing.png                      mountain valley photograph, 1920×1105
assets/scene/hero-glacier.jpg                 the same photo, graded cold and dark
assets/scene/water-reflection.jpg             flipped and blurred, for the water
assets/3d/profile.json, label.png             bottle silhouette and its printed mark
assets/fonts/firago-400/500/600.woff2         Latin + Georgian subset with the `case` feature
```

### What exists now, section by section

1. **Hero** — full-viewport. A graded glacier photograph on top, a waterline at 36% with three
   blurred wave paths drifting at different speeds, and below it the water: a `backdrop-filter`
   layer, two turbulence fields drifting as caustics, and rising bubbles. A real-time three.js
   glass bottle sits in the water: lathed from the silhouette of the actual 0.750 GLS render,
   with the printed mark drawn in its own render pass over the glass, a knurled navy cap and
   water filled to the shoulder. It rises out of the depth on load, then sways so the label stays
   facing the viewer; dragging spins it. The headline crosses the waterline, so its lower half is
   refracted by the water layer.
2. **Stats panel** — four figures on a glass panel overlapping the bottom of the hero.
3. **01 About** — two columns, the origin story, press release links, a framed photograph.
4. **02 The source** (light) — a drawn SVG cross-section: the landscape photographed on top,
   four strata below it, an aquifer glowing at the bottom, a dotted path with a droplet travelling
   down it, four numbered pins, and the four callouts in a row underneath.
5. **03 Products** (dark) — a horizontal rail of the seven bottles with case and pallet data,
   then the standards copy in two columns.
6. **04 Quality** (light) — the two SGS certificates with a lightbox, the bromate explanation,
   three cards with a shield, a Swiss flag and an EU flag.
7. **05 Partners** (dark) — a PSG photo mosaic, the lockup, the 2028 badge, the partnership copy,
   then a 4×2 logo wall where every logo is rendered as a white mark.
8. **06 FineWaters** (dark) — the laurel wreath around the FineWaters logo.
9. **Footer** — logo, navigation, address.

### What is weak and what I want from you

Pick the one section I name in my next message and rework only that one. For each change, explain
in one line why it is better, not what it does.

Known weak points, in order:
- The three.js glass still reads closer to white plastic than to glass at large sizes. Screen
  space transmission with a transparent canvas has little to refract. Suggest a concrete fix that
  keeps the canvas transparent over the page, or argue for a different approach.
- The source cross-section is drawn flat. It should feel like a physical slice of ground with
  depth and material, closer to an Icelandic Glacial style diorama, without becoming a toy block.
- Section transitions are abrupt: each band starts where the last one ends, with no overlap,
  no scroll-linked handover.
- The product rail has no sense of scale between a 0.330 glass bottle and a 5 litre PET.
- The typographic scale is uniform across sections; nothing earns a full-bleed moment.

### Output format

Return a unified diff against the file I paste, or the complete modified `index.html` if the
change is extensive. Change nothing outside the section I named. No commentary inside the code
beyond short comments where the reasoning is not obvious. End with a checklist of what you
verified: both languages, 375px, 1440px, reduced motion, no new dependencies.
