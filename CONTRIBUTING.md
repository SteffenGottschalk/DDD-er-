# Contributing — DDD-er-

Dieser Workflow trennt **Planung** und **Umsetzung**. Wir planen jede Änderung im Issue, bevor Code geschrieben wird. Es gibt keine Implementierung, die nicht aus einem geplanten Issue stammt.

## TL;DR

```
Idee → Issue (Plan, im Project, mit Size) → Diskussion → status:ready
     → Branch via `gh issue develop` (verknüpft mit Issue)
     → PR mit `Closes #<nr>` → Merge nach develop → Issue auto-close
     → Release: develop → main
```

Kein Code ohne Issue. Kein Issue ohne Plan. Kein Issue außerhalb des Projects. Kein Branch ohne Issue-Verknüpfung. Kein PR ohne Issue-Link.

LLM-Variante derselben Regeln: [`.github/llm/AGENTS.md`](.github/llm/AGENTS.md).

---

## 1. Issue anlegen (Planung)

```bash
gh issue create --template feature.yml   # Feature
gh issue create --template bug.yml       # Bug
gh issue create --template chore.yml     # Chore / Refactor / Docs
```

Pflicht im Issue:
- **Problem / Motivation** — warum machen wir das?
- **Scope** — was ist drin, was explizit nicht?
- **Umsetzungsplan** — Schritte, betroffene Komponenten.
- **Akzeptanzkriterien** — woran erkennen wir, dass es fertig ist?

Das Issue startet automatisch mit `status:planning`. Solange es so gelabelt ist, wird **nicht** implementiert — nur der Plan iteriert.

Direkt nach dem Anlegen pflegen:
- Labels: `type:`, `area:`, `status:planning` (Templates setzen viel automatisch).
- **Project**-Verknüpfung zu [@SteffenGottschalk/projects/1](https://github.com/users/SteffenGottschalk/projects/1) — übernimmt der Workflow `add-to-project.yml` automatisch.
- **Size**-Feld im Project (s/m/l/xl) — manuell in der Project-UI.
- Assignee + ggf. Milestone.

## 2. Labels

| Präfix      | Werte                                                              |
| ----------- | ------------------------------------------------------------------ |
| `type:`     | `feature`, `bug`, `refactor`, `chore`, `docs`, `test`              |
| `area:`     | `concept`, `docs`, `examples`, `infra`, `mehrere`                  |
| `priority:` | `high`, `medium`, `low` (optional)                                 |
| `status:`   | `planning`, `ready`, `in-progress`, `blocked`, `review`            |
| (sonstige)  | `epic` — größeres Vorhaben mit Sub-Issues                          |

Lifecycle:

```
planning → ready → in-progress → review → (closed)
                                  ↑
                              blocked (jederzeit)
```

```bash
gh issue edit 42 --add-label "status:ready" --remove-label "status:planning"
```

## 3. Project-Tracking

Jedes Issue gehört ins Project [@SteffenGottschalk/projects/1](https://github.com/users/SteffenGottschalk/projects/1) — keine Ausnahmen. Das Project ist die zentrale Sicht.

Status-Mapping:

| Trigger                              | Issue-Label              | Project-Status                            |
| ------------------------------------ | ------------------------ | ----------------------------------------- |
| Issue angelegt                       | `status:planning`        | `Todo`                                    |
| Plan steht, Size gesetzt             | `status:ready`           | `Ready for Develompent`                   |
| Branch via `gh issue develop`        | `status:in-progress`     | `In Progress`                             |
| PR geöffnet                          | `status:review`          | `In Progress` (+ verlinkter PR)           |
| Blockiert                            | `status:blocked`         | `Blocked`                                 |
| PR gemerged → Issue zu               | (entfernt, Issue closed) | `Done`                                    |

Statuswechsel passieren **bei jedem Übergang** — sonst ist das Board wertlos.

## 4. Branch anlegen

Erst wenn das Issue auf `status:ready` steht:

```bash
gh issue develop 42 --base develop --name feature/42-aggregate-pattern --checkout
gh issue edit 42 --add-label "status:in-progress" --remove-label "status:ready"
```

Naming (CI erzwingt das via `branch-name-check.yml`):
- `feature/<nr>-<slug>` für `type:feature`
- `bugfix/<nr>-<slug>` für `type:bug`
- `chore/<nr>-<slug>` für `chore` / `refactor` / `docs` / `test`
- `hotfix/<nr>-<slug>` für kritische Fixes direkt auf `main`

`<slug>`: kebab-case, ≤ 40 Zeichen.

## 5. Implementieren

- Bleib im Scope des Issues. Findest du beim Umsetzen etwas anderes — **neues Issue**, kein "while-I'm-here"-Refactor.
- Commits sprechend halten: `type(area): kurzbeschreibung` (z.B. `feat(concept): aggregate-root pattern`).
- Branch regelmäßig gegen `develop` rebasen, damit der PR sauber bleibt.

## 6. Pull Request

```bash
git push -u origin feature/42-aggregate-pattern
gh pr create --base develop --fill --label "status:review"
gh issue edit 42 --remove-label "status:in-progress" --add-label "status:review"
```

Im PR-Body **muss** stehen:
- `Closes #42` — schließt das Issue automatisch beim Merge. CI (`close-linked-issues.yml`) prüft das.
- Test Plan ausgefüllt.
- Akzeptanzkriterien aus dem Issue abgehakt.

## 7. Merge & Cleanup

- Merge nach `develop`.
- Issue schließt automatisch (`Closes #…`), Project-Item rutscht auf `Done`.
- Branch wird auf GitHub gelöscht.
- Lokal aufräumen: `git checkout develop && git pull && git branch -d feature/42-…`

## 8. Release (`develop` → `main`)

Sobald `develop` einen testbaren Stand erreicht hat:

```bash
gh pr create --base main --head develop --title "Release vYYYY-MM-DD" --body "Sammel-Release"
```

Nach Merge: ggf. Tag setzen (`git tag -a vX.Y.Z -m "..." && git push --tags`).

## 9. Hotfixes

Direkt auf `main`:

```bash
git checkout main && git pull
gh issue develop <nr> --base main --name hotfix/<nr>-slug --checkout
# Fix, PR --base main, Merge
git checkout develop && git merge main   # Backmerge
```

---

## Praktische `gh`-Snippets

```bash
# Alle offenen Planungs-Issues
gh issue list --label "status:planning"

# Was ist gerade in Arbeit?
gh issue list --label "status:in-progress"

# Ready-to-pick-up
gh issue list --label "status:ready"

# Eigene Issues
gh issue list --assignee "@me"

# Issue von planning → ready hochziehen
gh issue edit <nr> --add-label "status:ready" --remove-label "status:planning"

# Issue ins Project aufnehmen (falls Workflow mal nicht greift)
gh project item-add 1 --owner SteffenGottschalk \
  --url https://github.com/SteffenGottschalk/DDD-er-/issues/<nr>

# Branch anlegen und ans Issue verknüpfen
gh issue develop <nr> --base develop --name feature/<nr>-slug --checkout
```

## Epics

Für größere Vorhaben:

1. Epic-Issue mit Label `epic` und einer Checkliste verlinkter Sub-Issues.
2. Sub-Issues folgen dem normalen Workflow.
3. Epic schließt erst, wenn alle Sub-Issues erledigt sind.
