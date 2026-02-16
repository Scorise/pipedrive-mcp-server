# Custom Features - Pipedrive MCP Server

## 🎯 Dodane narzędzia

### `get-leads`
Pobiera wszystkie leady z Pipedrive (analogicznie do `get-deals`, `get-persons`).

**Parametry:**
- `ownerId` (optional) - Filtruj po owner ID
- `archivedStatus` (optional) - `'archived'` | `'not_archived'` | `'all'` (domyślnie: `'not_archived'`)
- `limit` (optional) - Limit wyników (domyślnie: 500)

**Zwraca:**
```json
{
  "summary": "Found X leads matching the specified filters",
  "filters_applied": {...},
  "total_found": X,
  "leads": [
    {
      "id": "...",
      "title": "...",
      "owner_name": "...",
      "person_name": "...",
      "add_time": "...",
      ...
    }
  ]
}
```

**Użycie w sales-analysis skill:**
```typescript
mcp__pipedrive__get-leads({
  archivedStatus: 'not_archived',
  limit: 500
})
```

---

## 🔧 Zarządzanie zmianami

### Struktura branchy:
- `main` - oryginalny kod z upstream
- `custom-features` - Twoje modyfikacje

### Aktualizacja z upstream:

```bash
# 1. Przełącz się na main
git checkout main

# 2. Pobierz zmiany z upstream (juhokoskela/pipedrive-mcp-server)
git fetch origin
git pull origin main

# 3. Przełącz się na custom-features
git checkout custom-features

# 4. Rebase zmian z main
git rebase main

# 5. Rozwiąż konflikty (jeśli są)
# Edytuj pliki z konfliktami
git add src/index.ts
git rebase --continue

# 6. Przebuduj
npm run build
```

### Aplikowanie patch (alternatywa):

Jeśli rebase się nie uda, użyj patch:

```bash
# 1. Zaktualizuj main
git checkout main
git pull origin main

# 2. Aplikuj patch
git apply ../pipedrive-get-leads.patch

# 3. Przebuduj
npm run build

# 4. Commit
git add src/index.ts
git commit -m "Apply get-leads patch"
```

---

## 📦 Repo info

- **Origin (Twój fork):** https://github.com/WillDent/pipedrive-mcp-server.git
- **Upstream (oryginalny):** https://github.com/juhokoskela/pipedrive-mcp-server.git
- **Branch z modyfikacjami:** `custom-features`
- **Commit:** `9fccd50` - "Add get-leads tool for fetching all leads from Pipedrive"

---

## 🔄 Restart serwera MCP

**Serwer działa w trybie stdio** (subprocess Claude Code), więc:

### ❌ NIE można zrestartować oddzielnie
```bash
# To NIE zadziała - brak osobnego procesu
ps aux | grep pipedrive
kill -9 <PID>
```

### ✅ RESTART wymaga:
1. Zamknij Claude Code (Ctrl+Q / Alt+F4)
2. Otwórz ponownie
3. Serwer zostanie automatycznie przeładowany

### ✅ Weryfikacja po restarcie:
```
ToolSearch: "pipedrive get-leads"
→ Powinno załadować mcp__pipedrive__get-leads
```

---

## 🚀 Pull Request do upstream (opcjonalnie)

Jeśli chcesz zaproponować `get-leads` do oficjalnego repo:

```bash
# 1. Upewnij się że jesteś na custom-features
git checkout custom-features

# 2. Push do swojego forka (WillDent/pipedrive-mcp-server)
git push origin custom-features

# 3. Utwórz Pull Request na GitHubie:
# https://github.com/WillDent/pipedrive-mcp-server/compare/main...custom-features
# Target: juhokoskela/pipedrive-mcp-server:main
# Source: WillDent/pipedrive-mcp-server:custom-features
```

**Tytuł PR:** "Add get-leads tool for fetching all leads"

**Opis:**
```markdown
## Summary
Adds `get-leads` tool to fetch all leads from Pipedrive, analogous to existing `get-deals`, `get-persons`, and `get-organizations` tools.

## Motivation
- Currently only `search-leads` exists, which requires a search term
- No way to fetch ALL leads for analysis (e.g., sales CRM audits)
- Inconsistent with other resources (deals/persons/orgs all have get-* methods)

## Changes
- Added `get-leads` tool in `src/index.ts`
- Supports filtering by: `ownerId`, `archivedStatus`, `limit`
- Returns summarized lead data (id, title, owner, person, org, timestamps)

## Testing
Tested with Pipedrive API - successfully fetches and filters leads.

## Use case
Enables sales-analysis workflows to fetch and analyze lead conversion rates, lead aging, and pipeline health.
```

---

**Utworzono:** 2026-02-16
**Wersja:** 1.0
**Autor:** Claude Sonnet 4.5 (custom modifications for Scorise)
