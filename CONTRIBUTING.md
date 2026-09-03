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

Format: `<Name> | <Kategorie>`. Repo-übergreifend identisch, gepflegt mit `apply-labels.sh` im Portfolio-Meta-Repo (Ort siehe dort `AGENTS.md`).

| Kategorie  | Werte                                                                                                                          |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `type`     | `Bug`, `Feature`, `Refactor`, `Chore`, `Docs`, `Test`                                                                          |
| `status`   | `Planning`, `Ready`, `In Progress`, `Review`, `Blocked`                                                                        |
| `priority` | `High`, `Medium`, `Low` (optional)                                                                                             |
| `area`     | `Backend`, `Frontend`, `API`, `Auth`, `Realtime`, `Storage`, `Infra`, `UX`, `Testing`                                          |
| `app`      | repo-spezifisch (DDD-er- hat aktuell keine)                                                                                    |
| `special`  | `Epic`                                                                                                                         |

Lifecycle:

```
Planning → Ready → In Progress → Review → (closed)
                                   ↑
                               Blocked (jederzeit)
```

```bash
gh issue edit 42 --add-label "Ready | status" --remove-label "Planning | status"
```

## 3. Project-Tracking

Jedes Issue gehört ins Project [@SteffenGottschalk/projects/1](https://github.com/users/SteffenGottschalk/projects/1) — keine Ausnahmen. Das Project ist die zentrale Sicht.

Status-Mapping:

| Trigger                              | Issue-Label              | Project-Status                            |
| ------------------------------------ | ------------------------ | ----------------------------------------- |
| Issue angelegt                       | `Planning | status`      | `Todo`                                    |
| Plan steht, Size gesetzt             | `Ready | status`         | `Ready for Development`                   |
| Branch via `gh issue develop`        | `In Progress | status`   | `In Progress`                             |
| PR geöffnet                          | `Review | status`        | `In Progress` (+ verlinkter PR)           |
| Blockiert                            | `Blocked | status`       | `Blocked`                                 |
| PR gemerged → Issue zu               | (entfernt, Issue closed) | `Done`                                    |

Statuswechsel passieren **bei jedem Übergang** — sonst ist das Board wertlos.

## 4. Branch anlegen

Erst wenn das Issue auf `Ready | status` steht:

```bash
gh issue develop 42 --base develop --name feature/42-aggregate-pattern --checkout
gh issue edit 42 --add-label "In Progress | status" --remove-label "Ready | status"
```

Naming (CI erzwingt das via `branch-name-check.yml`):
- `feature/<nr>-<slug>` — alles, was etwas hinzufügt oder ändert (`Feature`, `Refactor`, `Chore`, `Docs`, `Test`).
- `bugfix/<nr>-<slug>` — Fehler beheben. Von `develop` für normale Fixes, von `main` für kritische Hotfixes (danach Backmerge nach `develop`).

Andere Präfixe (`chore/`, `hotfix/`, …) sind **nicht erlaubt**.

`<slug>`: kebab-case, ≤ 40 Zeichen.

**Das Schema gilt für Themenzweige — nicht für Mainline-PRs.** Ein Release-PR
`develop` → `main` hat den Head-Ref `develop`, ein Hotfix-Backmerge
`main` → `develop` hat `main`; beide können das Schema nie erfüllen. Ebenso
benennt Dependabot seine Zweige selbst und kann daran nichts ändern. Alle drei
sind in `branch-name-check.yml` ausdrücklich ausgenommen, Dependabot zusätzlich
in `close-linked-issues.yml` (ein Bot legt kein Issue an, das er verlinken
könnte).

Bis 2026-09-03 fehlten diese Ausnahmen — mit Folgen, die in der Historie stehen:
Der vorgeschriebene Weg war rot, der direkte Push auf eine Mainline nicht.
PR #2 und #8 wurden mit rotem Namens-Check gemergt, der in diesem Abschnitt
verlangte Backmerge nach PR #6 fand nie statt, und `main` und `develop` liefen
auseinander. Siehe [#9](https://github.com/SteffenGottschalk/DDD-er-/issues/9).

## 5. Implementieren

- Bleib im Scope des Issues. Findest du beim Umsetzen etwas anderes — **neues Issue**, kein "while-I'm-here"-Refactor.
- Commits sprechend halten: `type(area): kurzbeschreibung` (z.B. `feat(concept): aggregate-root pattern`).
- Branch regelmäßig gegen `develop` rebasen, damit der PR sauber bleibt.

## 6. Pull Request

```bash
git push -u origin feature/42-aggregate-pattern
gh pr create --base develop --fill --label "Review | status"
gh issue edit 42 --remove-label "In Progress | status" --add-label "Review | status"
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

Kritische Fixes direkt auf `main` — Branch-Präfix bleibt `bugfix/`:

```bash
git checkout main && git pull
gh issue develop <nr> --base main --name bugfix/<nr>-slug --checkout
# Fix, PR --base main, Merge
git checkout develop && git merge main   # Backmerge
```

---

## Praktische `gh`-Snippets

```bash
# Alle offenen Planungs-Issues
gh issue list --label "Planning | status"

# Was ist gerade in Arbeit?
gh issue list --label "In Progress | status"

# Ready-to-pick-up
gh issue list --label "Ready | status"

# Eigene Issues
gh issue list --assignee "@me"

# Issue von Planning → Ready hochziehen
gh issue edit <nr> --add-label "Ready | status" --remove-label "Planning | status"

# Issue ins Project aufnehmen (falls Workflow mal nicht greift)
gh project item-add 1 --owner SteffenGottschalk \
  --url https://github.com/SteffenGottschalk/DDD-er-/issues/<nr>

# Branch anlegen und ans Issue verknüpfen (Bugs: --name bugfix/<nr>-slug)
gh issue develop <nr> --base develop --name feature/<nr>-slug --checkout
```

## Epics

Für größere Vorhaben:

1. Epic-Issue mit Label `Epic | special` und einer Checkliste verlinkter Sub-Issues.
2. Sub-Issues folgen dem normalen Workflow.
3. Epic schließt erst, wenn alle Sub-Issues erledigt sind.
