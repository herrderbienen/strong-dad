# Strong-DAD

Offline-fähige Trainings-App für René. **Eine einzige, selbst-enthaltene HTML-Datei**
(`index.html`) mit Vanilla-JavaScript – bewusst **kein React, kein CDN, kein Build-Tool**,
damit sie im Fitnessstudio auch ohne Netz zuverlässig läuft.

## Grundregeln (wichtig)
- **Alles in `index.html`**: Markup, CSS und JS in einer Datei. Keine externen Skripte/Styles.
- **Kein CDN, kein Framework.** Diagramme sind Inline-SVG (Funktion `chartSVG`), keine Chart-Bibliothek.
- **Bilder** sind als Base64-`data:`-URIs eingebettet (`const IMG = {…}`), damit offline verfügbar.
- **Nur `sd-icon.png`** liegt als echte Datei neben `index.html` (Home-Screen-Icon; `data:`-Icons
  werden von iOS beim „Zum Home-Bildschirm" oft nicht geladen).
- **Sprache der Oberfläche: Deutsch.**

## Datenhaltung (localStorage, pro Gerät)
| Key | Inhalt |
|-----|--------|
| `sd_sessions_v1` | Alle geloggten Einheiten |
| `sd_cycle_v1` | Aktueller Wellen-Schritt (Index) |
| `sd_timer_v1` | Default-Pausenzeit {min,sec} |
| `sd_owndraft_v1` | Entwurf „Eigenes Workout" |
| `sd_plan_v1` | Benutzer-Anpassungen am Plan (überlagert `DEFAULT_EX`) |

Helfer: `lget(key, fallback)` / `lset(key, value)`. Export/Import aller Keys:
Menü → **Backup / Umzug** (`makeBackup()` + Import schreibt die Keys und lädt neu).

## Zentrale Datenstrukturen (oben in der Datei)
- `CYCLE_STEPS` – Wellen-Periodisierung (1a/1b/2/3/dl, Faktoren 1.00/1.00/1.045/1.09/0.80);
  `liftForWeek(base, step)` + `roundHalf()` berechnen das Soll-Gewicht.
- `PLAN` – Tage `Di/Do/So`, je `{label, sub, ex:[…]}`. Übung:
  `{n, key, sets, reps, w?, base?, welle?, cue?, bw?, sec?}`. `key` matcht `SEED`-Keys, wo Historie existiert.
- `DEFAULT_EX` – tiefe Kopie des Standardplans beim Start (für „Auf Standard zurücksetzen").
- `SEED` – importierte Historie der Haupt-Lifts (Baseline für die Progressionskurven).
- `NAME_MAP` – Namens-Mapping für den Strong-App-Import.

## Views (Funktion `render(view)`)
`menu · plan · log · planedit · prog · cond · warm · own · imp · backup`.
Im `log`/`own`-View sitzt der Pausen-Timer in der **sticky Topbar** (bleibt beim Scrollen oben).

## Features im Log-View
- ⋮-Menü je Übung: Notiz, nach oben/unten verschieben (persistiert im Plan), Überspringen.
- **Steiger-Hinweis** (`dueIncrease`): Badge, wenn letztes Mal in allen Sätzen die obere
  Wdh-Grenze erreicht wurde; Tippen füllt alle Sätze mit +2,5 kg (Beine/Deadlift/Hip Thrust +5 kg).
  **Nicht** bei Wellen-Lifts. Entscheidung bleibt beim Nutzer.
- „Einheit speichern & abschließen" → Zusammenfassung (Volumen, schwerster Satz, neue 1RM-Bestwerte).

## Änderungen testen (vor jedem Release)
1. JS extrahieren und syntaktisch prüfen: `node --check`.
2. Render-/Interaktionstest mit **jsdom** (alle Views rendern, ⋮-Menü, Steigern, Backup-Roundtrip)
   müssen ohne Fehler durchlaufen.
Polyfills im Test nötig: `getContext`, `scrollTo`, `navigator.vibrate`, `AudioContext`;
jsdom mit `url:"https://…"` starten (sonst kein localStorage).

## Hosting
Statisch. `index.html` + `sd-icon.png` ins Repo-Root → GitHub Pages (Branch `main`, `/root`).
Aufruf: `https://<user>.github.io/<repo>/`. Danach in **Safari** öffnen → Teilen →
„Zum Home-Bildschirm". Kein Server → **keine** geräteübergreifende Sync (dafür später NAS).

## Nicht-Ziele / bewusst weggelassen
- Keine Server-Logik, keine Accounts, kein Tracking.
- Keine Fremd-Bibliotheken. Wenn etwas „nur mit Lib ginge" → erst Rückfrage.
