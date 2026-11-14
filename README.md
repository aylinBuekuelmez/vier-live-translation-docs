# VIER Live Translation – Dokumentationswebsite

Dieses Repository enthält die **öffentliche** Projektdokumentation. Keine API-Schlüssel, Zugangsdaten oder ungeprüften internen Inhalte einfügen.

## Dateien bearbeiten

Die Webseiteninhalte liegen als Markdown in `docs/`. Textdateien auf GitHub öffnen, auf den Stift klicken, Änderungen speichern (`Commit changes`). Änderungen auf `main` starten die Veröffentlichung automatisch.

## Lokal testen (optional)

Python 3.11+ installieren, danach:

```bash
pip install -r requirements.txt
mkdocs serve
```

Die Vorschau ist dann unter http://127.0.0.1:8000 erreichbar.

## Veröffentlichung

In GitHub `Settings > Pages > Build and deployment > Source: GitHub Actions` wählen. `.github/workflows/deploy.yml` baut und veröffentlicht die Seite bei Pushes auf `main`.

## Dokumentationsprozess

Confluence bleibt interne Wissensbasis. Nur freigegebene Inhalte in dieses öffentliche Repository übertragen. Quellen, Tabellen, Abbildungen und Links nach dem Export kontrollieren.
