# Task planen

Erstelle eine neue geplante Aufgabe für das Projekt.

## Ablauf

1. **Eingabe erfassen** — Frage nach: Taskname, Typ (feature/bugfix/refactor/docs/infra), Ziel, Beschreibung. Falls der User diese Infos bereits mitgegeben hat, nicht erneut fragen.

2. **Task-Datei erstellen** — Kopiere das Template aus `.github/templates/plan-task.md` nach `plan/<taskname>.md` (kebab-case, z.B. `plan/auth-system.md`). Fülle die Felder aus den gesammelten Infos aus.

3. **Subtasks ableiten** — Leite aus Ziel und Beschreibung sinnvolle Subtasks ab. Jeder Subtask soll eine klar abgrenzbare Arbeitseinheit sein.

4. **Akzeptanzkriterien definieren** — Formuliere messbare Kriterien, wann die Aufgabe als abgeschlossen gilt.

5. **Zielversion setzen** — Für Features: nächste Minor-Version. Für Bugfixes: nächste Patch-Version. Aktuelle Version aus `package.json` lesen.

6. **Plan-Übersicht aktualisieren** — Füge einen Eintrag in `plan/README.md` unter "Backlog" ein:
   ```
   - [ ] Taskname (nicht freigegeben) — [plan/taskname.md](taskname.md)
   ```

7. **Zusammenfassung** — Zeige dem User die erstellte Task-Datei und frage, ob Anpassungen nötig sind.

## Regeln

- Keine Implementierung starten — nur planen und dokumentieren
- Dateien in `plan/` verwenden kebab-case ohne Präfix
- Typ "bugfix" → Patch-Version, alles andere → Minor-Version
