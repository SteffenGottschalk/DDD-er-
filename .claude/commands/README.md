# Slash-Befehle

Sechs Standard-Befehle (Quelle: `Ity Tools` Monorepo, deutsch):

- `/plan-task` — neue geplante Aufgabe in `plan/` anlegen
- `/implement` — geplanten Task aufnehmen und umsetzen
- `/test` — Tests schreiben oder ausführen
- `/docs` — Dokumentation nach Änderung pflegen
- `/dev-setup` — lokale Entwicklungsumgebung aufsetzen
- `/complete-feature` — Feature/Bugfix sauber abschließen (lint, types, tests, build)

## Stack-Annahmen

Die Befehle gehen von einem **pnpm + Vitest + Next.js**-Stack aus. DDD-er- hat aktuell
keinen solchen Stack — die Befehle sind **Templates für später**. Sobald DDD-er- Code bekommt:
- Pfade (`docs/__tests__/`, `plan/`) prüfen oder anpassen.
- Build-Befehle (`pnpm lint`, `pnpm test:types`, `pnpm test`, `pnpm build`) gegen den
  tatsächlichen Stack tauschen.

## Workflow-Bezug

Diese Befehle ergänzen den Issue/PR-Workflow aus
[`../../CONTRIBUTING.md`](../../CONTRIBUTING.md), sie ersetzen ihn **nicht**: Auch wenn
`/plan-task` lokal eine Datei in `plan/` erzeugt, gilt nach wie vor "kein Code ohne
GitHub-Issue + Project-Verknüpfung".
