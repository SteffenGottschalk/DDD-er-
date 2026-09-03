# Lessons Learned — DDD-er-

Kurze Lehren aus konkreten Vorfällen. **Vor jeder neuen Aufgabe lesen** — Score
≥ 6 zuerst.

## Format

```
## <Bereich>
- YYYY-MM-DD [N/10] — <Lehre in einem Satz>.
  <Warum / Konsequenz, 1–2 Sätze, optional>
```

**Bedeutungstiefe (N/10):**

- **10** — Datenverlust, Produktionsausfall, Sicherheitsproblem
- **8–9** — schwerer Vorfall, Revert nötig, signifikanter Zeitverlust
- **6–7** — verlässlicher Stolperstein, leicht reproduzierbar
- **4–5** — nützlicher Hinweis, gelegentlich relevant
- **2–3** — Nice-to-know, selten
- **1** — Anekdote

**Cleanup:** Wenn diese Datei > 50 Einträge bekommt, alle mit Score ≤ 3 prüfen
und ggf. fusionieren oder löschen. Quartalsweise.

**Cross-Repo-Lehren** (gelten in mehreren Repos) gehören nicht hierher, sondern
in die **Lessons-Ablage des Portfolio-Meta-Repos** — dort unter dem Namen
`lessons-learned`; der Ort steht in dessen `AGENTS.md`. Hier nur, was
repo-spezifisch ist.

> Der Weg dorthin wird **beim Namen genannt und nicht als Pfad geschrieben.** Die
> ursprüngliche Vorlage zu diesem Issue verwies auf
> `Brain/knowledge/lessons-cross-repo.md`. Dieses Repo ist **gelöscht** — nicht
> archiviert, wie ältere Portfolio-Dokumente sagen: API und Web geben am
> 2026-09-03 beide **HTTP 404**. Der Link wäre also von der ersten Minute an tot
> gewesen. Genau dieselbe Lehre hat PR #8 in dieses Repo getragen: Der Name ist
> stabil, der Ort nicht.

---

## CI / Workflows

- 2026-09-03 [8/10] — Ein Gate, das den vorgeschriebenen Weg ablehnt, erzieht dazu, ihn zu umgehen.
  `branch-name-check.yml` verlangte `feature|bugfix/<nr>-<slug>` und traf damit
  auch jeden Mainline-PR: Ein Release-PR `develop`→`main` hat den Head-Ref
  `develop`, der in `CONTRIBUTING.md` §4 vorgeschriebene Hotfix-Backmerge hat
  `main`. Beide konnten nie grün werden. Folge: Es wurde direkt auf die
  Mainlines gearbeitet, `main` und `develop` liefen um je einen Commit
  auseinander — in denselben zwei Dateien. Behoben in #9. **Wer ein Gate baut,
  prüft es gegen die eigenen dokumentierten Abläufe, nicht nur gegen den
  Normalfall.**

- 2026-09-03 [7/10] — `required_status_checks: null` heißt: Die Checks sind sichtbar, aber nicht bindend.
  Auf beiden Mainlines war kein Check erforderlich. Deshalb konnten PR #2
  (`chore/1-foundation`) und PR #8 (`docs/7-meta-pfad`) mit **rotem** Namens-Check
  gemergt werden, ohne dass es jemandem auffiel. Ein rotes Häkchen, das nichts
  verhindert, ist Dekoration.

- 2026-09-03 [6/10] — Ein Bot kann zwei dieser Gates grundsätzlich nicht erfüllen.
  Dependabot benennt seine Zweige selbst (`dependabot/…`) und legt kein Issue an,
  das es per `Closes #<nr>` verlinken könnte. PR #4 stand deshalb ab dem Tag
  seiner Entstehung (2026-05-05) dauerhaft auf rot. Ausnahmen hängen am **Actor**
  (`dependabot[bot]`), nicht am Zweignamen — sonst wären sie eine Umgehung für
  jeden.

## Git / Branch-Modell

- 2026-09-03 [7/10] — GitHub schließt verlinkte Issues nur beim Merge in den **Default-Branch**.
  PR #6 trug `Closes #5` und wurde nach `main` gemergt; Default-Branch ist hier
  `develop`. Das Issue blieb offen, und niemand suchte den Grund im Zielzweig.
  Gegenprobe am selben Tag: PR #8 → `develop` schloss #7 sofort. **Wer einen
  Hotfix auf `main` mergt, schließt das Issue von Hand oder liefert den
  vorgeschriebenen Backmerge nach.**

- 2026-09-03 [6/10] — Bei einem Zweig, der auseinandergelaufen ist, ist die Frage nicht „gibt es Konflikte?", sondern „was verschwindet?".
  Der Zwei-Punkt-Diff `develop`↔`main` meldete **56 Löschungen**, der
  Drei-Punkt-Diff nur 2 Zeilen Beitrag. Die 56 waren kein Rückbau, sondern das,
  was `main` **fehlte**. Entschieden hat erst der Ergebnisbaum
  (`git merge-tree --write-tree`, verglichen mit `develop`): genau zwei Zeilen.
  **Am Ergebnis messen, nicht am Diff der Absicht.**

## Doku

- 2026-09-03 [5/10] — Ein absoluter Pfad in ein fremdes Repo veraltet, sobald dort etwas umzieht.
  Zwei Dokuzeilen nannten `_foundation/apply-labels.sh` im Portfolio-Meta-Repo;
  dessen Meta-Ebene zog nach `ai/`, und beide Zeilen zeigten ins Leere — in
  sieben Repos gleichzeitig. Behoben in #7/PR #8, indem der **Fremdpfad ganz
  entfiel**: Werkzeug beim Namen, Ort beim Meta-Repo.

## Stack / Build

_(noch keine Einträge — das Repo hat bislang keinen Sprach-Stack)_

## Sonstiges

_(noch keine Einträge)_
