# Playground

The Playground is an interactive SQL console for your platform data. Use it to explore your data model, prototype queries, and inspect results — without touching any production API.

<p align="center">
  <img src="../../images/playground.png" alt="Playground — SQL editor, entity tree and results" width="90%">
</p>

## Layout

The Playground has three columns:

**Entity tree (left)** — a collapsible tree of all entities in your active configuration. Click a category to expand it and see individual objects. Right-click an entity name to copy it into the editor, or drag it directly.

**SQL editor (center)** — a Monaco code editor with Ledgyx SQL syntax. Write your query here. Below the main editor is a smaller JSON params editor for passing query parameters.

**Results + History (right)** — query results appear at the top (as a table if the result is an array, or as JSON otherwise). Below the results is a scrollable history of all past queries.

## Running a query

1. Write an Ineron SQL query in the editor (see [Ineron SQL Reference](../ineron-sql.md))
2. Press **Run** (or `Ctrl+Enter`) to execute
3. Results appear in the right panel

Every query is saved to history automatically.

## Working with results

When a query returns a list:
- Results display as a sortable table
- Click any row to open a **detail view** with all fields rendered with type-aware formatting (numbers in violet, booleans in amber, objects expandable)
- Use `← Table` to go back and `‹ N / total ›` arrows to navigate between rows
- **Export** button downloads the result as JSON

## Query history

The history panel shows all your past queries for the active configuration:
- **Search** — filter history by query text
- **Infinite scroll** — older queries load automatically as you scroll down
- Click any history item to load it into the editor

## Persistence

Your current query and parameters are remembered between page reloads (saved in browser storage). Use the **Clear** button (eraser icon) to reset the editor and results.

## Default query

The editor starts with:
```sql
SELECT 1 TYPE OBJECT;
```
This is a quick health check — it should always return `{"?column?": 1}`.

## Tips

- Drag an entity name from the tree directly into the editor to insert it at the cursor position.
- Use the **From history** picker in Events to bring a query you developed in Playground directly into an event method — click the history icon next to the method tabs.
- The entity tree is shared with the Entities page — changes you make there appear here immediately.
