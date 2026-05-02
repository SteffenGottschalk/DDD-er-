# Dokumentation aktualisieren

Aktualisiere die Projektdokumentation nach einer Änderung.

## Ablauf

1. **Änderungen verstehen** — Lies die aktuellen Git-Änderungen:
   ```bash
   git diff --stat HEAD~1
   git log --oneline -5
   ```

2. **Betroffene Dokumente identifizieren** — Prüfe, welche Dokumente aktualisiert werden müssen:

   | Änderung an | Dokument aktualisieren |
   |---|---|
   | Tech-Stack, Dependencies | `README.md` (Tech-Stack) |
   | Projektstruktur, neue Ordner | `README.md` (Projektstruktur) |
   | Persistenz, Datenmodell | `README.md` (Persistenz) |
   | Build, Scripts, Deployment | `README.md` (Entwicklung, Deployment) |
   | Arbeitsregeln, Konventionen | `AGENTS.md` |
   | Agent-Instruktionen | `CLAUDE.md` |
   | Neue Features, Roadmap | `plan/README.md` |

3. **Dokumente aktualisieren** — Nur Fakten ändern, keine spekulativen Ergänzungen. Dokumentation soll den IST-Stand beschreiben.

4. **Konsistenz prüfen** — Alle Dokumente müssen zueinander passen:
   - Version in README = Version in package.json
   - Skripte in README = Skripte in package.json
   - Struktur in README = tatsächliche Ordnerstruktur

## Regeln

- Dokumentation ist deutsch
- Knapp und präzise schreiben
- Keine Emojis
- Keine Duplikation zwischen README und AGENTS.md
- CLAUDE.md bleibt kurz — verweist auf AGENTS.md für Details
