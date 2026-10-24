# Celina & Lukas – Hochzeits-Onepager

Eine einzelne HTML-Datei, für Handys gebaut (die Gäste scannen den QR-Code), ohne Build-Tools.
Design orientiert an der Einladung: dunkles Bordeaux, Creme, Goldakzente, florale Illustration.

## Dateien
```
lina-luki-wedding-neu/
  index.html          ← die ganze Seite (CSS und JS inline)
  qr-code.svg         ← QR-Code für die Einladungskarten (unverändert)
  img/hero-bloom.svg  ← floraler Hero-Hintergrund (Rosen + Blattwerk, dark bordeaux)
  img/botanical.svg   ← goldene Linien-Botanik für die Ränder der hellen Abschnitte
  img/monogram.svg    ← Siegel/Monogramm C & L (Navigation, Ort-Abschnitt, Footer)
  img/favicon.svg     ← Tab-Symbol
  img/rose.svg        ← Einzelrose (Motivbaukasten, z. B. für Druckstücke)
  img/sprig.svg       ← Blattzweig (Motivbaukasten)
  Beispiel1.jpeg      ← Designreferenz (Einladung) – nur Vorlage, kein Seiten-Asset
  Moodboard.jpeg      ← Designreferenz (Moodboard) – nur Vorlage, kein Seiten-Asset
```

Die ehemaligen Foto-Platzhalter (`img/hero.svg`, `couple.svg`, `venue.svg`, `map.svg`,
`gallery-1..3.svg`) sind entfernt – die Seite zeigt bewusst keine erfundenen Bilder.
Echte Fotos später: als `img/*.jpg` ablegen und die Stelle im Abschnitt „Unsere Geschichte“
(bzw. im Hero) ergänzen.

## Anschauen
Doppelklick auf `index.html`. Es läuft alles lokal, ohne Server.
Zum Testen des Formularversands über einen Endpunkt ist ein kleiner Server nötig
(`python -m http.server`, weil `fetch` per `file://` blockiert wird).

## Verbindliche Angaben (Stand heute)
| Was | Wert |
| --- | --- |
| Namen | Celina & Lukas, Kurzform Lina & Luki |
| Termin | Samstag, 30. September 2028 |
| Region | Bayern · Oberfranken · Landkreis Bayreuth |
| Rückmeldefrist | 30. September 2027 |
| Kontakt | Celina, +49 162 7909083 (`tel:+491627909083`) |
| Uhrzeit, Location, Adresse | **noch nicht bestätigt** – im Text überall „folgt“ |

Diese Werte stehen an genau zwei Stellen: sichtbar im HTML und unsichtbar im
`<script>`-Objekt `CONFIG` (Countdown, E-Mail-Betreff, Nachrichtentext).

## RSVP-Versand – noch zu erledigen
Das Formular sammelt die Rückmeldung personenbezogen (Name, Zu-/Absage, Menü,
Ernährungsbedürfnisse je Person) plus Übernachtung, Anreisetag und Freitext.
Der Versand ist noch **nicht** eingerichtet – bewusst, weil die Empfängeradresse
nicht feststeht. In `CONFIG` (unten in `index.html`) gibt es zwei Schalter:

1. `endpoint: "/api/rsvp"` – empfohlen. Das Formular sendet dann `POST` als JSON:
   ```json
   {
     "hochzeit": "Celina & Lukas – Samstag, 30. September 2028",
     "rueckmeldung_am": "24.10.2026, 09:30:29",
     "antwort": "Zusage", "person": "Max Mustermann", "e_mail": "max@example.org",
     "anzahl_personen": 2, "anzahl_zusagen": 1,
     "gaeste": [
       { "nummer": 1, "vorname": "Max", "nachname": "Mustermann", "name": "Max Mustermann",
         "teilnahme": "nimmt teil", "essen": "Vegetarisch",
         "ernaehrungsbeduerfnisse": "glutenfrei" },
       { "nummer": 2, "vorname": "Anna", "nachname": "Mustermann", "name": "Anna Mustermann",
         "teilnahme": "sagt ab" }
     ],
     "uebernachtung": "Ja", "anreise": "Freitag, 29. September 2028",
     "nachricht": "Wir kommen mit dem Zug."
   }
   ```
   Der Endpunkt muss `POST` mit `Content-Type: application/json` annehmen und mit
   `200/201` antworten. Mögliche Dienste: eigenes Skript, Formspark, Formspree,
   Zapier-Webhook – oder ein kleiner Server am Hoster.
2. `email: "rsvp@…"` – ohne Backend: Das Mailprogramm des Gastes öffnet sich mit der
   fertig getexteten Nachricht. Versand gilt erst nach dem Klick auf „Senden“.

Ist beides leer, **werden keine Daten übertragen**: Die Seite sagt das offen und zeigt
die fertige Antwort zum Kopieren. Der Hinweis erscheint automatisch auch über dem Formular.

Offene Punkte, bevor die Seite live geht:
- [ ] Empfänger-E-Mail **oder** Backend-Endpunkt in `CONFIG` eintragen
- [ ] Genaue Location, Adresse und Uhrzeit bestätigen und überall ergänzen
- [ ] Persönliche Geschichte im Abschnitt „Unsere Geschichte“ schreiben (Platz ist vorbereitet)
- [ ] Geprüfte Unterkunftsempfehlungen in „Übernachtung“ ergänzen
- [ ] Eigene Fotos einfügen
- [ ] `Impressum`/`Datenschutz`: Der Datenschutzhinweis liegt als Text im Formular.
      Für eine öffentliche Domain zwei kurze Seiten anlegen und die Links im Footer setzen.
- [ ] Domain klären (`qr-code.svg` zeigt auf `https://lina-luki-wedding.de`)

## Das Rückmeldeformular
Abschnitte: Wer antwortet (Vor-/Nachname, E-Mail = Pflichtfeld) · Zu- oder Absage ·
Personen (jede einzeln, eigene Zu-/Absage, Menü mit Fleisch/Vegetarisch/Vegan,
Glutenfrei/Laktosefrei/Keine Nüsse/Keine Meeresfrüchte) · Übernachtung (Ja/Nein) ·
Anreisetag (Freitag, 29.09.2028 oder Samstag, 30.09.2028) · „Noch etwas?“ (optional) ·
Zustimmung. Bei einer Absage verschwinden Essen, Übernachtung und Anreise automatisch.
Die erste Personenkarte übernimmt den Namen aus dem ersten Abschnitt automatisch,
sobald dort getippt wird – wer sie selbst bearbeitet, behält die eigene Eingabe.

## Barrierefreiheit & Technik
Responsiv ab 320 px, Sprunglink zum Inhalt, Feldgruppen mit Legend, sichtbare
Fokusringe, `aria-live` für Fehler und Ergebnis, `prefers-reduced-motion` wird respektiert,
Honeypot gegen Spam, keine Frameworks, keine externen Skripte (nur Google Fonts).

## Druck für die Karten
`qr-code.svg` ist Vektor – für Karten mit ~20 mm QR-Kante direkt als SVG ins Layout
(InDesign/Illustrator/Affinity) legen. Schwarze Module auf Weiß, mit Randzone.
`img/rose.svg`, `img/sprig.svg` und `img/monogram.svg` passen gestalterisch zur Website,
falls die gedruckte Karte dieselbe Bildsprache bekommen soll.
