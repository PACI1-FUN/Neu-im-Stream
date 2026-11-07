# Neu im Stream – Streamingdienst hinzufügen oder löschen

Alle Streamingdienste stehen an **einer einzigen Stelle** in der Datei `index.html`:
in der Liste `PLATFORMS` (ca. Zeile 200–210). Sonst muss nichts geändert werden –
Filter-Knöpfe, Abzeichen und Willkommenstext passen sich automatisch an.

So sieht die Liste aus:

```js
const PLATFORMS = [
  { key: "netflix", label: "Netflix",  color: "#e50914", re: /^netflix/i },
  { key: "disney",  label: "Disney+",  color: "#1a4bdb", re: /^disney/i },
  { key: "sky",     label: "Sky",      color: "#0a7bd6", re: /^(sky(?!\s*store)|wow\b)/i },
  { key: "apple",   label: "Apple TV", color: "#6e6e73", re: /^apple tv(?!\s*store)/i },
];
```

Jede Zeile ist ein Dienst:

| Feld    | Bedeutung                                                                 | Beispiel            |
|---------|---------------------------------------------------------------------------|---------------------|
| `key`   | Interner Kurzname: klein geschrieben, ohne Leerzeichen, nur einmal vorhanden | `"prime"`           |
| `label` | Name, der in der App angezeigt wird                                        | `"Prime Video"`     |
| `color` | Farbe des Abzeichens (Hex-Farbcode)                                        | `"#00a8e1"`         |
| `re`    | Suchmuster für den Namen des Dienstes bei TMDB                             | `/^amazon prime video/i` |

Zum Bearbeiten genügt ein einfacher Texteditor (auf dem Mac z. B. **TextEdit** im Modus
„Format → In reinen Text umwandeln“ oder besser der kostenlose **Visual Studio Code**).

---

## A) Einen Dienst löschen

1. `index.html` im Texteditor öffnen.
2. In der Liste `PLATFORMS` die ganze Zeile des Dienstes löschen, z. B. für Sky:
   ```js
     { key: "sky",     label: "Sky",      color: "#0a7bd6", re: /^(sky(?!\s*store)|wow\b)/i },
   ```
3. Speichern – fertig. Weiter mit **C) Testen und online stellen**.

> Achte darauf, dass die Liste mit `const PLATFORMS = [` beginnt und mit `];` endet und
> jede Zeile mit `},` abgeschlossen ist.

---

## B) Einen Dienst hinzufügen

### Schritt 1: Genauen Namen des Dienstes bei TMDB herausfinden

Die App erkennt einen Dienst an seinem Namen bei TMDB. So findest du ihn:

1. Diese Adresse im Browser öffnen – dabei `DEIN_SCHLUESSEL` durch deinen TMDB-API-Schlüssel
   ersetzen (den kurzen, ca. 32 Zeichen) und bei Bedarf `CH` durch `DE` oder `AT`:
   ```
   https://api.themoviedb.org/3/watch/providers/tv?watch_region=CH&language=de-DE&api_key=DEIN_SCHLUESSEL
   ```
2. Mit **⌘ + F** nach dem Dienst suchen (z. B. „Prime“). Wichtig ist der Text hinter
   `"provider_name"`, z. B. `"provider_name": "Amazon Prime Video"`.
3. Es gibt oft mehrere Einträge (z. B. „… with Ads“, „… Channel“). Das Muster in Schritt 2
   erwischt alle Einträge, die **gleich beginnen** – das ist meist gewünscht.

Alternative ohne Schlüssel: auf [themoviedb.org](https://www.themoviedb.org) einen Film
öffnen, der auf dem Dienst läuft, und unter „Wo ansehen / Where to Watch“ den Namen ablesen.

### Schritt 2: Zeile in die Liste einfügen

Vor der Zeile `];` eine neue Zeile einfügen, z. B. für Amazon Prime Video:

```js
  { key: "prime",   label: "Prime Video", color: "#00a8e1", re: /^amazon prime video/i },
```

Oder für Paramount+:

```js
  { key: "paramount", label: "Paramount+", color: "#0064ff", re: /^paramount/i },
```

Regeln für das Muster `re`:

- `/^netflix/i` bedeutet: „Name **beginnt mit** netflix“, Gross-/Kleinschreibung egal.
- Mehrere mögliche Namen mit `|` trennen und in Klammern setzen:
  `/^(sky|wow)/i` = beginnt mit „sky“ **oder** „wow“.
- Einen Namen ausschliessen mit `(?!...)`:
  `/^sky(?!\s*store)/i` = beginnt mit „sky“, aber **nicht** „Sky Store“ (Kauf/Miete).
- Das Muster muss am Anfang `/^` und am Ende `/i` haben.

> Die App zeigt nur Titel aus dem **Abo** des Dienstes (Flatrate), keine Kauf- oder
> Leihfilme. Reine Kauf-/Leihshops (z. B. „Sky Store“, „Apple TV Store“) bringen also nichts.

### Schritt 3: Speichern

Datei speichern. Weiter mit **C)**.

---

## C) Testen und online stellen

1. **Lokal testen:** `index.html` per Doppelklick im Browser öffnen.
   Der neue Dienst erscheint oben als Filter-Knopf, ein gelöschter ist verschwunden.
   (Die App lädt nach einer Änderung an der Liste automatisch neu – der 3-Stunden-Zwischenspeicher
   wird dabei übergangen.)
2. **Neuen Dienst prüfen:** Auf seinen Knopf klicken. Erscheint „…wurde in diesem Zeitraum nichts
   Neues veröffentlicht“, ist entweder wirklich nichts Neues erschienen oder das Muster passt nicht
   zum TMDB-Namen → Schritt B1 nochmals prüfen. Zum Gegencheck in den Einstellungen den Zeitraum
   auf „Letzte 30 Tage“ stellen.
3. **Weisse/leere Seite?** Dann ist beim Bearbeiten ein Tippfehler passiert (meist ein fehlendes
   Komma, Anführungszeichen oder `}`). Die geänderte Zeile mit den Beispielen oben vergleichen.
4. **Online stellen (GitHub):** Im Repository `neu-im-stream` auf **Add file → Upload files**,
   die geänderte `index.html` hineinziehen und **Commit changes** klicken.
   Nach 1–2 Minuten ist die neue Version online; auf dem iPhone die App einmal schliessen und
   neu öffnen (ggf. nach unten ziehen zum Neuladen).

---

## Kurzübersicht

| Ich möchte …              | Was tun                                                         |
|---------------------------|-----------------------------------------------------------------|
| Dienst löschen            | Seine Zeile in `PLATFORMS` löschen → speichern → hochladen       |
| Dienst hinzufügen         | TMDB-Namen suchen → neue Zeile vor `];` → speichern → hochladen |
| Nur Name/Farbe ändern     | `label` bzw. `color` in der Zeile ändern                        |
| Land wechseln (CH/DE/AT)  | Kein Code nötig: in der App unter ⚙︎ Einstellungen                |
