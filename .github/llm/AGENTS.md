# AGENTS.md — DDD-er-

LLM-Briefing für dieses Repo. Gilt für **alle** Assistenten (Claude Code, Cursor, Copilot, Gemini).
Menschliche Variante: [`CONTRIBUTING.md`](../../CONTRIBUTING.md).

## Goldene Regel

> **Kein Code ohne Issue. Kein Branch ohne Issue-Nummer. Kein PR ohne `Closes #<nr>`.**

## 1. Workflow auf einen Blick

```
Idee → gh issue create  →  Issue im Project (Status: Todo, "Planning | status")
                       ↘   Plan ausarbeiten, Size setzen
                          → Label "Ready | status" (Project: Ready for Development)
                          → gh issue develop <nr>            (Branch verknüpft)
                          → "In Progress | status" (Project: In Progress)
                          → Commits, push, gh pr create --base develop
                          → "Review | status"
                          → Merge nach develop → Issue auto-closed (Done)
                          → Release: develop → main
```

## 2. Branch-Modell (GitFlow-light)

- `main` — Release, immer deploybar. Default-Branch.
- `develop` — Integration, hier wird gesammelt + getestet.
- `feature/<nr>-<slug>` — alles, was etwas hinzufügt oder ändert (Feature, Refactor, Chore, Docs, Test). Von `develop`, zurück nach `develop`.
- `bugfix/<nr>-<slug>` — Fehler beheben. Von `develop` für normale Fixes, von `main` für kritische Hotfixes (danach Backmerge nach `develop`).

Andere Präfixe (`chore/`, `hotfix/`, …) sind **nicht erlaubt**. Der CI-Check `branch-name-check.yml` erzwingt das Schema.

Branches **immer** mit `gh issue develop <nr> --name <branch>` anlegen — das verknüpft den Branch mit dem Issue.

Slug-Regel: kebab-case, ≤ 40 Zeichen, alphanumerisch + `-`.

## 3. Issue-Disziplin

Issues entstehen ausschließlich über `gh` — niemals direkt im Browser ohne Project-Verknüpfung.

```bash
gh issue create --template feature.yml   # bzw. bug.yml / chore.yml
```

Pflicht-Labels (Templates setzen viel automatisch). Format: `<Name> | <Kategorie>`.

| Kategorie  | Werte                                                                                                                          |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `type`     | `Bug`, `Feature`, `Refactor`, `Chore`, `Docs`, `Test`                                                                          |
| `status`   | `Planning` → `Ready` → `In Progress` → `Review` → (geschlossen). `Blocked` jederzeit.                                          |
| `priority` | `High`, `Medium`, `Low` (optional)                                                                                             |
| `area`     | `Backend`, `Frontend`, `API`, `Auth`, `Realtime`, `Storage`, `Infra`, `UX`, `Testing`                                          |
| `app`      | repo-spezifische Apps/Komponenten — DDD-er- hat aktuell keine                                                                  |
| `special`  | `Epic` für größere Vorhaben mit Sub-Issues                                                                                     |

Statuswechsel:
```bash
gh issue edit <nr> --remove-label "Planning | status" --add-label "Ready | status"
```

Das Label-Set ist **repo-übergreifend identisch** (Farben + Namen), gepflegt via `_foundation/apply-labels.sh`.

Jedes Issue gehört ins Project [@SteffenGottschalk/projects/1](https://github.com/users/SteffenGottschalk/projects/1) — der `add-to-project.yml`-Workflow fügt neue Issues automatisch ein.
**Size-Feld** (s/m/l/xl) wird vor `status:ready` in der Project-UI gesetzt.

## 4. Commits

`type(area): kurzbeschreibung`. Beispiele:
- `feat(concept): aggregate-root invariant docs`
- `fix(docs): typo im README`
- `chore(infra): bump actions/checkout v6`

## 5. Pull Requests

```bash
gh pr create --base develop --fill --label "Review | status"
```

PR-Body **muss** enthalten:
- `Closes #<nr>` — sonst `close-linked-issues.yml`-Check schlägt fehl.
- Test Plan ausgefüllt.
- Out-of-Scope-Check abgehakt.

Ziel-Branch:
- `feature/*` → `develop`
- `bugfix/*` → `develop` (für Bugs in develop) oder `main` (für kritische Hotfixes; danach manueller Backmerge `main` → `develop`)

## 6. Was *nicht* getan werden darf

- Direkt auf `main` oder `develop` committen.
- Branches ohne Issue oder mit freiem Naming anlegen.
- "While-I'm-here"-Refactors einschmuggeln — neues Issue, separater PR.
- Existierende `.claude/commands/`-Dateien überschreiben, ohne den Plan im Issue zu erwähnen.
- Tooling-Configs (Workflows, Templates) ohne `chore`-Issue ändern.

## 7. `gh`-Schnellzugriff

```bash
# Was ist offen?
gh issue list --label "Planning | status"
gh issue list --label "Ready | status"
gh issue list --label "In Progress | status"
gh issue list --assignee "@me"

# Issue ins Projekt nachtragen (sollte normalerweise der Workflow machen)
gh project item-add 1 --owner SteffenGottschalk \
  --url https://github.com/SteffenGottschalk/DDD-er-/issues/<nr>

# Status hochziehen
gh issue edit <nr> --remove-label "Ready | status" --add-label "In Progress | status"

# Branch + Issue-Verknüpfung anlegen (Bugfixes: --name bugfix/<nr>-slug)
gh issue develop <nr> --base develop --name feature/<nr>-slug --checkout
```

## 8. Verhalten gegenüber dem Repo

- **Vor Code:** betroffene Dateien lesen, Annahmen im Issue prüfen.
- **Bei Unklarheit:** im Issue nachfragen (Comment), nicht raten.
- **Nach Änderungen:** Doku im selben PR aktualisieren (README, dieses File, falls relevant).
- **Niemals** `git push --force` auf `main` oder `develop`.
- **Niemals** `--no-verify`. Bei Hook-Fail: Ursache fixen, neuer Commit.

## 9. Spezifika DDD-er-

DDD-er- ist ein Konzept-Skelett für Domain-Driven-Design-Material. Aktuell kein Build-Stack
— Issues drehen sich um Konzepte, Beispiele, Doku. Sobald Code dazukommt, wird dieses
Dokument um Stack-Notes (Test-/Build-Befehle) erweitert.
