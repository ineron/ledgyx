# Ineron SQL Reference

Ledgyx uses **Ineron SQL** — a custom SQL dialect built on top of standard SQL that is optimized for working with the platform's entity-attribute model. You write Ineron SQL in the Playground and in Event handlers.

## Basic syntax

Every query must end with a `TYPE` declaration:

```sql
SELECT 1 TYPE OBJECT;
```

The `TYPE` clause tells the platform how to interpret and return the result:

| Type | Returns |
|---|---|
| `TYPE OBJECT` | A single JSON object |
| `TYPE LIST` | An array of JSON objects |

## Querying entities

```sql
-- List all products
SELECT
  p.ref() AS id,
  p.title,
  p.price,
  p.description
FROM Dictionary.Product AS p
TYPE LIST;
```

```sql
-- Get a single product by ref
SELECT
  p.ref() AS id,
  p.title,
  p.price
FROM Dictionary.Product AS p
WHERE p.ref() = &params.id::uuid
TYPE OBJECT;
```

## Using parameters

Parameters come from the HTTP request body. Use `&body` for the full body or `&params.field` for a specific field:

```sql
-- Insert a new product (POST handler)
INSERT INTO Dictionary.Product (title, price, description)
VALUES (&body.title, &body.price::NUMBER, &body.description)
TYPE OBJECT;
```

```sql
-- Update (PUT handler)
UPDATE Dictionary.Product
SET title = &body.title, price = &body.price::NUMBER
WHERE ref() = &body.id::uuid
TYPE OBJECT;
```

```sql
-- Delete (DELETE handler)
DELETE FROM Dictionary.Product
WHERE ref() = &params.id::uuid
TYPE OBJECT;
```

## Supported type casts

Only these casts are supported in Ineron SQL:

| Cast | Use for |
|---|---|
| `::uuid` | UUID values |
| `::text` | Text / string values |
| `::NUMBER` | Numeric values (integer and decimal) |

> **Do not use** `::integer`, `::boolean`, `::date`, `::varchar` — these are not supported and will cause a syntax error.

## Reserved words

Some words must always be in **double quotes** when used as field names:

```sql
-- "comment" is a reserved word
SELECT p.title, p."comment" FROM Dictionary.Post AS p TYPE LIST;
```

Other common reserved words: `type`, `name`, `value`, `order`, `group`, `user`.

## The ref() function

Every entity instance has a unique immutable reference. Use `ref()` to select it:

```sql
SELECT p.ref() AS id, p.title FROM Dictionary.Product AS p TYPE LIST;
```

You can filter by ref:
```sql
WHERE p.ref() = &params.id::uuid
```

## CALL providers

Event handlers can call platform services using the `CALL` statement:

### Run an AI agent
```sql
SELECT CALL(
  AGENT "my_agent"
  FROM &body
  NEXT "reply_event"
  POST
) TYPE OBJECT;
```

### Call an LLM directly
```sql
SELECT CALL(
  AI_CHAT_COMPLETION "openai_creds"
  AI_SCHEMAS "my_schema"
  FROM (SELECT &body.text AS message)
  NEXT "process_result"
  POST
) TYPE OBJECT;
```

### Send a Telegram message
```sql
SELECT CALL(
  TG_SEND_MESSAGE "my_bot"
  FROM (SELECT &body.text AS message, &body.chat_id AS chat_id)
) TYPE OBJECT;
```

### Download and process a file from Telegram
```sql
SELECT CALL(
  TG_GET_FILE "my_bot"
  AI_SCHEMAS "ocr_schema"
  FROM &body
  NEXT "handle_result"
  POST
) TYPE OBJECT;
```

### Call an external webhook
```sql
SELECT CALL(
  WEBHOOK "stripe_hook"
  FROM &body
  NEXT "webhook_result"
  GET
) TYPE OBJECT;
```

### Push a real-time update via SSE
```sql
SELECT CALL(
  SSE_SEND "my_sse_token"
  FROM (SELECT &body.message AS text)
) TYPE OBJECT;
```

## Tips

- The platform **automatically filters out deleted records** (state=9) — you don't need to add `WHERE state = 1` in your queries.
- Fields named `comment`, `type`, `name` must always be double-quoted.
- Date literals are not directly supported — use the platform's built-in `now()` for current timestamp.
- The Playground shows errors inline — a great place to develop and test queries before wiring them to events.
