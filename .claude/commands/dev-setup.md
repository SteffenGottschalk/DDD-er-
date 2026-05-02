# Entwicklungsumgebung einrichten

Führe durch die lokale Einrichtung der Entwicklungsumgebung.

## Ablauf

1. **Voraussetzungen prüfen** — Stelle sicher, dass installiert ist:

   ```bash
   node --version   # muss Node 22 LTS sein
   pnpm --version   # muss pnpm 10.x sein
   ```

   Falls nicht vorhanden, Installationshinweise geben.

2. **Dependencies installieren**

   ```bash
   pnpm install
   ```

3. **Umgebungsvariablen** — Prüfe, ob `.env` existiert. Falls nicht:

   ```bash
   cp .env.example .env
   ```

   Für lokale Entwicklung Basic Auth leer lassen.

4. **Dev-Server starten**

   ```bash
   pnpm dev
   ```

   Der Server startet unter `http://localhost:3000`. Er beinhaltet:
   - Next.js App mit Hot Reload
   - WebSocket-Server für Live-Synchronisation
   - Automatische Port-Wahl falls 3000 belegt

5. **Verifikation** — Prüfe, ob alles funktioniert:

   ```bash
   pnpm lint && pnpm test:types && pnpm build
   ```

6. **Projektstruktur erklären** — Gib eine kurze Übersicht:
   - `src/app/` — Next.js App Router, Seiten und API-Routen
   - `src/components/` — React-Komponenten
   - `src/lib/` — Typen, State-Management, Hilfsfunktionen
   - `src/backend/` — Custom Node-Server mit WebSocket
   - `tests/` — Vitest-Tests
   - `plan/` — Aufgabenplanung
   - `data/` — Serverseitige JSON-Daten (nicht committed)
