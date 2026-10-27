# Strong-DAD

Eine kleine, offline-fähige Trainings-App – eine einzige HTML-Datei, kein Server, keine Anmeldung.
Plan (Di/Do/So) mit Wellen-Periodisierung, Satz-Logging mit Pausen-Timer, Progressionskurven,
Aufwärm- und Conditioning-Übersichten.

## Nutzen / Hosting
Die App ist statisch – `index.html` + `sd-icon.png` reichen.

**GitHub Pages:** Beide Dateien ins Repo-Root legen → *Settings → Pages → Branch `main` / `/root`*.
Danach erreichbar unter `https://<user>.github.io/<repo>/`.

**Als App aufs iPhone:** die Seite in **Safari** öffnen, komplett laden lassen →
Teilen-Symbol → **„Zum Home-Bildschirm"**. Startet dann im Vollbild mit eigenem Icon.

## Daten & Backup
Alle Einträge liegen **lokal im Browser** des jeweiligen Geräts (kein Cloud-Speicher).
Umzug/Backup: Menü → **Backup / Umzug** → *Kopieren* (oder *Als Datei*), auf dem neuen
Gerät unter *Wiederherstellen* einfügen → *Importieren & neu laden*.

## Technik
Vanilla JavaScript, kein Framework, kein CDN. Diagramme als Inline-SVG, Bilder als
eingebettete Base64-Daten. Details für Weiterentwicklung siehe `CLAUDE.md`.

## Hinweis
Nur für den privaten Gebrauch gedacht.
