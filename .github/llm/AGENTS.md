# AGENTS.md — DDD-er-

LLM-Briefing für dieses Repo. Gilt für **alle** Assistenten (Claude Code, Cursor, Copilot, Gemini).
Menschliche Variante: [`CONTRIBUTING.md`](../../CONTRIBUTING.md).

## Goldene Regel

> **Kein Code ohne Issue. Kein Branch ohne Issue-Nummer. Kein PR ohne `Closes #<nr>`.**

## 1. Workflow auf einen Blick

```
Idee → gh issue create  →  Issue im Project (Status: Todo, status:planning)
                       ↘   Plan ausarbeiten, Size setzen
                          → Label status:ready (Project: Ready for Develompent)
                          → gh issue develop <nr>            (Branch verknüpft)
                          → status:in-progress (Project: In Progress)
                          → Commits, push, gh pr create --base develop
                          → status:review
                          → Merge nach develop → Issue auto-closed (Done)
                          → Release: develop → main
```

## 2. Branch-Modell (GitFlow-light)

- `main` — Release, immer deploybar. Default-Branch.
- `develop` — Integration, hier wird gesammelt + getestet.
- `feature/<nr>-<slug>` — neue Funktionalität, von `develop`, zurück nach `develop`.
- `bugfix/<nr>-<slug>` — Fehler in `develop` beheben.
- `hotfix/<nr>-<slug>` — kritischer Fix direkt auf `main`, danach Backmerge nach `develop`.

Branches **immer** mit `gh issue develop <nr> --name <branch>` anlegen — das verknüpft den Branch mit dem Issue.

Slug-Regel: kebab-case, ≤ 40 Zeichen, alphanumerisch + `-`. Ein CI-Check (`branch-name-check.yml`) erzwingt das.

## 3. Issue-Disziplin

Issues entstehen ausschließlich über `gh` — niemals direkt im Browser ohne Project-Verknüpfung.

```bash
gh issue create --template feature.yml   # bzw. bug.yml / chore.yml
```

Pflicht-Labels (Templates setzen viel automatisch):
- `type:` — `feature`, `bug`, `chore`, `refactor`, `docs`, `test`
- `area:` — siehe Issue-Template-Dropdown (`concept`, `docs`, `examples`, `infra`, `mehrere`)
- `status:` — `planning` → `ready` → `in-progress` → `review` → (closed). `blocked` jederzeit.

Statuswechsel:
```bash
gh issue edit <nr> --remove-label status:planning --add-label status:ready
```

Jedes Issue gehört ins Project [@SteffenGottschalk/projects/1](https://github.com/users/SteffenGottschalk/projects/1) — der `add-to-project.yml`-Workflow fügt neue Issues automatisch ein.
**Size-Feld** (s/m/l/xl) wird vor `status:ready` in der Project-UI gesetzt.

## 4. Commits

`type(area): kurzbeschreibung`. Beispiele:
- `feat(concept): aggregate-root invariant docs`
- `fix(docs): typo im README`
- `chore(infra): bump actions/checkout v6`

## 5. Pull Requests

```bash
gh pr create --base develop --fill --label "status:review"
```

PR-Body **muss** enthalten:
- `Closes #<nr>` — sonst `close-linked-issues.yml`-Check schlägt fehl.
- Test Plan ausgefüllt.
- Out-of-Scope-Check abgehakt.

Ziel-Branch:
- `feature/*`, `bugfix/*`, `chore/*` → `develop`
- `hotfix/*` → `main` (danach manueller Backmerge `main` → `develop`)

## 6. Was *nicht* getan werden darf

- Direkt auf `main` oder `develop` committen.
- Branches ohne Issue oder mit freiem Naming anlegen.
- "While-I'm-here"-Refactors einschmuggeln — neues Issue, separater PR.
- Existierende `.claude/commands/`-Dateien überschreiben, ohne den Plan im Issue zu erwähnen.
- Tooling-Configs (Workflows, Templates) ohne `chore`-Issue ändern.

## 7. `gh`-Schnellzugriff

```bash
# Was ist offen?
gh issue list --label "status:planning"
gh issue list --label "status:ready"
gh issue list --label "status:in-progress"
gh issue list --assignee "@me"

# Issue ins Projekt nachtragen (sollte normalerweise der Workflow machen)
gh project item-add 1 --owner SteffenGottschalk \
  --url https://github.com/SteffenGottschalk/DDD-er-/issues/<nr>

# Status hochziehen
gh issue edit <nr> --remove-label status:ready --add-label status:in-progress

# Branch + Issue-Verknüpfung anlegen
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
