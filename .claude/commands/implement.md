# Task implementieren

Nimm eine geplante Aufgabe aus `plan/` auf und implementiere sie.

## Ablauf

1. **Task auswählen** — Wenn kein Task angegeben: Lies `plan/README.md`, zeige die freigegebenen offenen Tasks und lass den User wählen. Nur freigegebene Tasks (`(freigegeben)`) dürfen implementiert werden.

2. **Task-Datei lesen** — Lies die zugehörige Datei in `plan/` und verstehe Ziel, Subtasks und Akzeptanzkriterien.

3. **Status aktualisieren** — Setze den Status in der Task-Datei auf "in Arbeit".

4. **Architektur prüfen** — Vor dem Coding:
   - Betroffene Dateien identifizieren und lesen
   - Abhängigkeiten und Seiteneffekte prüfen
   - Bei Unklarheiten Rückfragen stellen

5. **Subtasks abarbeiten** — Implementiere jeden Subtask einzeln. Nach jedem Subtask:
   - Checkbox in der Task-Datei abhaken
   - Kurz prüfen, ob Lint und test:types noch durchlaufen

6. **Tests** — Schreibe oder aktualisiere Tests für die geänderte Funktionalität:
   - Tests liegen co-located unter `docs/__tests__/` neben dem Modul
   - Komponenten und Module immer isoliert testen
   - Komplexe Logik in eigene Dateien extrahieren (z.B. `calculations.ts`), damit sie ohne React-Context testbar ist
   - Beispiel: `src/lib/weeks.ts` → `src/lib/docs/__tests__/weeks.test.ts`

7. **Verifikation** — Führe die vollständige Prüfung aus:

   ```bash
   pnpm lint && pnpm test:types && pnpm test && pnpm build
   ```

8. **Task abschließen** — Wenn alle Subtasks und Akzeptanzkriterien erfüllt:
   - Nutze `/complete-feature` für den Abschluss

## Regeln

- Nur freigegebene Tasks implementieren
- Immer zuerst lesen, dann coden
- Keine Änderungen über den Scope des Tasks hinaus
- Bei Problemen: Task-Datei mit Notizen ergänzen, nicht stillschweigend Scope ändern
