# Tests schreiben und ausführen

Schreibe oder führe Tests für das Projekt aus.

## Ablauf

### Tests ausführen

```bash
pnpm test          # alle Tests
pnpm test:watch    # Watch-Modus für Entwicklung
```

### Tests schreiben

1. **Betroffene Dateien identifizieren** — Lies den Quellcode, der getestet werden soll.

2. **Testdatei anlegen** — Tests liegen co-located bei den Modulen unter `docs/__tests__/`:
   - `src/lib/weeks.ts` → `src/lib/docs/__tests__/weeks.test.ts`
   - `src/lib/holidays.ts` → `src/lib/docs/__tests__/holidays.test.ts`
   - `src/components/allocation-cell.tsx` → `src/components/docs/__tests__/allocation-cell.test.tsx`

3. **Test-Struktur** — Verwende Vitest:
   ```typescript
   import { describe, it, expect } from 'vitest'

   describe('Modulname', () => {
     it('sollte X tun wenn Y', () => {
       // Arrange
       // Act
       // Assert
     })
   })
   ```

4. **Was testen:**
   - **Lib-Funktionen:** Reine Logik, Berechnungen, Transformationen
   - **API-Routen:** Request/Response-Verhalten
   - **Komponenten:** Nur bei komplexer Logik (nicht reines Rendering)
   - **Nicht testen:** Triviale Wrapper, reine UI-Layouts, Framework-Verhalten

5. **Tests ausführen und Ergebnis prüfen**
   ```bash
   pnpm test
   ```

## Regeln

- Tests co-located bei den Modulen unter `docs/__tests__/`
- Dateinamen: `*.test.ts` oder `*.test.tsx`
- Keine Mocks für interne Module, wenn vermeidbar
- Imports verwenden `@/` Alias wie im Produktionscode
