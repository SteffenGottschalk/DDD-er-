# Feature abschließen

Schließe ein implementiertes Feature oder einen Bugfix sauber ab.

## Ablauf

1. **Prüfungen ausführen** — Alle vier Checks müssen bestehen:

   ```bash
   pnpm lint
   pnpm test:types
   pnpm test
   pnpm build
   ```

   Bei Fehlern: zuerst beheben, dann erneut prüfen. Nicht fortfahren, bis alles grün ist.

2. **Version erhöhen** — Lies die aktuelle Version aus `package.json`:
   - **Feature/Refactor/Infra/Docs:** Minor-Bump (0.8.0 → 0.9.0)
   - **Bugfix:** Patch-Bump (0.8.0 → 0.8.1)
     Den Typ aus der Task-Datei in `plan/` ablesen, falls vorhanden.

3. **Task-Datei aktualisieren** — Falls eine Task-Datei in `plan/` existiert:
   - Status auf "abgeschlossen" setzen
   - Alle Subtask-Checkboxen prüfen
   - Abschluss-Checkliste abhaken

4. **Plan-Übersicht aktualisieren** — In `plan/README.md`:
   - Eintrag von "Backlog" nach "Abgeschlossen" verschieben
   - Checkbox auf `[x]` setzen

5. **Commit erstellen** — Staged alle relevanten Dateien und erstelle einen Commit:
   - Format: `feat: <Beschreibung>` (Feature) oder `fix: <Beschreibung>` (Bugfix)
   - Version im Commit erwähnen: `feat: add auth system (v0.9.0)`

6. **Zusammenfassung** — Zeige: neue Version, bestandene Checks, Commit-Hash.

## Regeln

- NIEMALS ohne bestandene Checks committen
- NIEMALS `--no-verify` verwenden
- Version wird NUR in `package.json` erhöht (kein Tag, kein Changelog)
- Nicht automatisch pushen — der User entscheidet, wann gepusht wird
