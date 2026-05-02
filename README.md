# DDD-er-

DDD[er] — Domain Driven Design[er]. Konzept-Repo für DDD-Material, Beispiele und Dokumentation.

## Status

Skelett. Keine Code-Artefakte, kein Build-Stack. Inhalte werden über Issues geplant
(siehe [`CONTRIBUTING.md`](CONTRIBUTING.md)).

## Mitarbeiten

Workflow strikt Issue-first:

1. Idee → `gh issue create --template feature.yml` (oder `bug.yml` / `chore.yml`).
2. Plan iterieren, Label auf `Ready | status` setzen, Size im Project pflegen.
3. Branch via `gh issue develop <nr> --base develop --name feature/<nr>-slug --checkout` (Bugs: `bugfix/<nr>-slug`). Andere Präfixe sind nicht erlaubt.
4. PR `--base develop` mit `Closes #<nr>` öffnen.
5. Release: `develop` → `main`.

Volle Anleitung: [`CONTRIBUTING.md`](CONTRIBUTING.md).
Tracking: [@SteffenGottschalk/projects/1](https://github.com/users/SteffenGottschalk/projects/1).

## For AI Agents

Briefing für LLM-basierte Assistenten (Claude Code, Cursor, GitHub Copilot, Gemini, …)
liegt in [`.github/llm/`](.github/llm/). Einstieg:
[`.github/llm/AGENTS.md`](.github/llm/AGENTS.md). Slash-Befehle für Claude Code
unter [`.claude/commands/`](.claude/commands/).

## Lizenz

[MIT](LICENSE)
