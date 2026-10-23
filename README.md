# Lina & Luki – Hochzeits-Onepager

Eine einzelne HTML-Datei, für Handys gebaut (die Gäste scannen den QR-Code), ohne Backend.

## Dateien
```
lina-luki-wedding/
  index.html      ← die ganze Seite (CSS und JS inline, keine Build-Tools)
  qr-code.svg     ← QR-Code auf https://lina-luki-wedding.de für die Einladungskarten
  img/hero.svg        Platzhalter Paarfoto    (1200×800)
  img/couple.svg      Platzhalter Lina & Luki (900×1100)
  img/venue.svg       Platzhalter Location    (1200×800)
  img/map.svg         Platzhalter Karte       (1200×700)
  img/gallery-1..3.svg Platzhalter Galerie    (je 900×640)
```

## Anschauen
Doppelklick auf `index.html` (oder `Start-Process index.html`). Es läuft alles lokal, ohne Server.
Für den echten Betrieb später: Ordner auf einen beliebigen Hoster legen (Netlify, All-inkl, Hetzner, GitHub Pages), Domain `lina-luki-wedding.de` darauf verbinden.

## Platzhalter ersetzen
1. **Datum & Ort** – im `index.html` unten im Skript bei `const CONFIG`:
   - `weddingDate` (z. B. `"2027-08-14T15:00:00+02:00"`) → steuert den Countdown
   - `email` → Empfänger der Rückmeldungen, solange kein Backend läuft
   - `endpoint` → Backend-URL, sobald der E-Mail-Versand steht
   - `rsvpDeadline` → Rückmeldetermin
   Dieselben Werte stehen auch zweimal im sichtbaren Text (Hero und FAQ) – dort steht jeweils
   `<!-- ▼ PLATZHALTER -->` darüber.
2. **Bilder** – echte Fotos als `img/hero.jpg`, `img/couple.jpg`, `img/venue.jpg`, `img/map.jpg`,
   `img/gallery-1.jpg` … legen und in `index.html` den `src` von `.svg` auf `.jpg` ändern.
   Dateinamen und Maße bleiben gleich, Markup sonst unverändert.
3. **Texte** – alle Absätze, die mit `PLATZHALTER` beginnen, sind nur Fülltext.
   Suche im File nach `PLATZHALTER` (44 Stellen) und ersetze sie.

## Das Rückmeldeformular
Felder: Name · E-Mail · Zu-/Absage · Erwachsene & Kinder · Essen (Standard/Vegetarisch/Vegan +
Allergene als Chips) · Übernachtung ja/nein (+ Unterkunft und Nächte, erscheinen nur bei „Ja“) ·
Anreise · Shuttle/Mitfahrt · Anmerkungen · Zustimmungs-Checkbox · Button **Absenden**.
Bei „Absage“ verschwinden die Planungsfelder automatisch.

Der Absenden-Button ist bewusst als Platzhalter gebaut:
- Ohne `CONFIG.endpoint` öffnet sich das Mailprogramm des Gastes mit der fertig zusammengebauten
  Nachricht an `CONFIG.email` (`mailto:`) – es passiert also schon etwas Sinnvolles.
- Mit `CONFIG.endpoint` (z. B. `/api/rsvp`, Formspark, Zapier, eigenes Skript) wird per `POST` als
  JSON gesendet und die Danke-Anzeige gezeigt. Ein Server ist dafür noch nicht geschrieben.
- Immer wird die Antwort unter der Danke-Anzeige angezeigt und kann kopiert werden – falls der
  Versand fehlschlägt, geht nichts verloren.

## Druck für die Karten
`qr-code.svg` ist Vektor – für Karten mit ~20 mm QR-Kante direkt als SVG ins Layout
(InDesign/Illustrator/Affinity) legen. Schwarze Module auf Weiß, mit Randzone: scanbar bis
ca. 12 mm. Falls der QR farbig werden soll, bleibt der Kontrast hoch (`#3d5749` auf Weiß).

## Rechtliches
`Impressum` und `Datenschutz` im Footer sind noch leere Links – für eine echte Domain müssen
die zwei Seiten ergänzt werden (bei einer privaten Hochzeitsseite reicht ein kurzes Impressum).
