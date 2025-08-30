We’re excited to introduce Ledgyx – a new platform for flexible API creation using SQL! 🚀  
Beta testing is now live, and we invite everyone to join.  

🔹 SQL instead of complex GraphQL queries  
🔹 Flexible data structure with zero downtime  
🔹 No-Code API – set up in minutes  
🔹 Web3 support & blockchain data analysis  
🔹 Solution marketplace  

# Custom SQL Language Guide

## Key Differences from Standard SQL

### 1. Mandatory Result Type Declaration
Every query MUST end with result type specification:

**Built-in types:**
- `TYPE OBJECT` - single record
- `TYPE LIST` - multiple records  
- `TYPE API OBJECT` - API object
- `TYPE API LIST` - API list

**Custom types:**
- `TYPE CustomName[]` - custom list type
- `TYPE ANY[]` - generic list

```sql
SELECT name, id FROM dictionary.users TYPE LIST;
SELECT name FROM dictionary.users WHERE id = 1 TYPE OBJECT;
```

### 2. Object Type Prefixes
System uses different object types with distinct syntax and properties:

- `Dictionary.` - data dictionaries/references (standard CRUD operations)
- `Contract.` - blockchain smart contracts (read-only, special functions)
- `Operation.` - operational/transactional data (business logic)
- `Ethereum.` - Ethereum blockchain access (external data source)
- `Enum.` - enumerations (predefined value sets)
- `Const.` - system constants (global configuration)

Each type has unique capabilities:
```sql
-- Dictionary: standard table operations
SELECT * FROM Dictionary.users WHERE id = 1 TYPE LIST;

-- Contract: blockchain data with parse functions  
SELECT parse(addr, topics, data) FROM Contract.token TYPE LIST;

-- Ethereum: external blockchain queries with parameters
FROM Ethereum.transactions.log(WHERE "to"=&address LIMIT 100) as trn

-- Enum: dot notation access
SELECT enum.status.active

-- Const: global values
SELECT const.api_url TYPE OBJECT;
```

### 3. Query Parameters
Parameters passed with `&` symbol:
```sql
WHERE user_id = &userId::NUMBER
WHERE name = &userName::TEXT  
WHERE ref = &userRef::UUID
WHERE address = &contractAddress::HEX
WHERE created > &startDate::DATE('DD.MM.YYYY')
```

**Parameter types:**
- `::NUMBER` - numbers
- `::TEXT` - text
- `::UUID` - UUID
- `::HEX` - hex addresses  
- `::DATE('format')` - dates with format

### 4. External Data Structures
Data can be passed as structures:
```sql
FROM &data as d(id int, name text, value numeric)
FROM &params as x(ref uuid, id int, name text, value text)
```

### 5. Dictionary Creation
```sql
CREATE DICTIONARY IF NOT EXISTS users (
    id NUMERIC NOT NULL,
    name TEXT(100),
    address DICTIONARY.address FIELD,  -- reference to another dictionary
    PRIMARY KEY (id)
);
```

### 6. Operation Tables
```sql
CREATE OPERATION IF NOT EXISTS operation_order(
    goods DICTIONARY.users FIELD NOT NULL,
    storage DICTIONARY.acl FIELD NOT NULL,
    value NUMERIC(12,2) FIELD NOT NULL
);
```

### 7. Constants
```sql
CREATE CONST.main_url TYPE TEXT;
UPDATE CONST SET main_url = "https://example.com";
SELECT const.main_url TYPE OBJECT;
```

### 8. Enumerations
```sql
CREATE ENUM IF NOT EXISTS digits AS (one, two, three);
SELECT enum.digits.one;
```

### 9. Nested Subqueries as Objects
```sql
SELECT user.name,
(SELECT addr.city, addr.street 
 FROM dictionary.address as addr 
 WHERE addr.id = user.id TYPE OBJECT) as address
FROM dictionary.users as user TYPE LIST;
```

### 10. Blockchain Functions
```sql
-- Parse transaction data
SELECT parse(&contractAddress, trn.topics, trn.data) as parsed_data
FROM Ethereum.transactions.log(WHERE "to"=&contractAddress LIMIT 100) as trn

-- External API calls
SELECT call(TG "tg_bot_config", msg.message) as result
FROM (SELECT &message as message TYPE OBJECT) as msg TYPE OBJECT;
```

### 11. Special Functions
- `uuid(field1, field2)` - generate UUID
- `now()` - current time
- `NULLREF()` - null reference
- `parse(address, topics, data)` - parse blockchain data
- `call(service, params)` - external service call

### 12. Array/Object Access
```sql
-- Array element access
SELECT &returnValues[0].token0, &returnValues."0", &raw.topics[1]

-- Object field access
SELECT &returnValues.sender, &params.ref
```

### 13. INSERT with Conflicts
```sql
INSERT INTO dictionary.users(id, name)
SELECT x.id, x.name 
FROM &params as x(id int, name text) 
ON CONFLICT(ref) DO UPDATE SET name=x.name, id=EXCLUDE.id;
```

### 14. JOIN with Blockchain Data
```sql
FROM Contract.uniswapv2factory as uv
INNER JOIN ethereum.transactions.data(WHERE "to"=uv.pair LIMIT 1) as trn ON true
```

### 15. DELETE Operations
```sql
-- Regular delete
DELETE FROM dictionary.users WHERE ref = &ref::UUID;

-- Permanent delete
DELETE PERMANENTLY FROM dictionary.events WHERE id = 100;
```

### 16. Quoted Reserved Words
Fields with reserved words use double quotes:
```sql
SELECT sm."event", sm."type", p."object"
```

### 17. Comparison and Logic Operators
```sql
-- Comparison: =, <>, !=, <, >, <=, >=
-- Logic: AND, OR, NOT
-- Special: LIKE, REGEXP, IN, EXISTS, BETWEEN

WHERE field = value AND (field2 IN (1,2,3) OR field3 LIKE '%pattern%')
WHERE EXISTS(SELECT 1 FROM other_table WHERE condition)
WHERE field BETWEEN min_val AND max_val
```

### 18. CASE Expressions
```sql
SELECT CASE 
  WHEN condition1 THEN 'value1'
  WHEN condition2 THEN 'value2' 
  ELSE 'default_value' 
END as result_field

-- Simple CASE form
SELECT CASE field_value
  WHEN 1 THEN 'one'
  WHEN 2 THEN 'two'
  ELSE 'other'
END as text_value
```

### 19. System Functions
```sql
-- Date/time
SELECT CURRENT_TIMESTAMP, CURRENT_DATE, CURRENT_TIME
SELECT now()
SELECT version()

-- String functions  
SELECT SUBSTRING(field FROM 1 FOR 10)
SELECT TRIM(LEADING ' ' FROM field)

-- Aggregate functions
SELECT COUNT(*), COUNT(field)

-- Conditional functions
SELECT ISNULL(field1, field2, 'default') -- COALESCE equivalent
```

### 20. Type Casting (CAST)
```sql
-- General syntax
SELECT field::TYPE_NAME
SELECT &param::UUID, &value::NUMBER, &text::TEXT

-- Special date syntax
SELECT '2024-01-01'::DATE('YYYY-MM-DD')
```

### 21. Interval Expressions
```sql
SELECT DATE_ADD(date_field, INTERVAL 1 DAY)
SELECT DATE_SUB(date_field, INTERVAL 1 HOUR)

-- Supported intervals:
-- DAY, HOUR, MINUTE, SECOND, YEAR, MONTH
-- DAY_HOUR, DAY_MINUTE, DAY_SECOND, HOUR_MINUTE, HOUR_SECOND
-- YEAR_MONTH, DAY_MICROSECOND, HOUR_MICROSECOND
```

### 22. Arithmetic and Bitwise Operations
```sql
SELECT field1 + field2, field1 - field2, field1 * field2, field1 / field2
SELECT field1 % field2, field1 MOD field2
SELECT field1 & field2, field1 | field2, field1 ^ field2  -- bitwise
SELECT field1 << 2, field1 >> 1  -- shifts
```

### 23. GROUP BY and ORDER BY
```sql
SELECT field1, COUNT(*)
FROM table_name
GROUP BY field1 
HAVING COUNT(*) > 1
ORDER BY field1 ASC, field2 DESC
LIMIT 10 OFFSET 20
```

### 24. Subqueries in Various Places
```sql
-- In SELECT
SELECT (SELECT COUNT(*) FROM other_table WHERE condition) as count_field

-- In WHERE with ANY/ALL/SOME
WHERE field = ANY(SELECT field FROM other_table)
WHERE field > ALL(SELECT field FROM other_table)

-- Existence checks
WHERE EXISTS(SELECT 1 FROM other_table WHERE condition)
```

## Common Query Examples

### Data Selection
```sql
SELECT u.name, u.id 
FROM Dictionary.users as u 
WHERE u.name LIKE &pattern::TEXT 
LIMIT &limit TYPE LIST;
```

### Data Insertion
```sql
INSERT INTO Dictionary.users(ref, id, name)
SELECT uuid(d.id, d.name), d.id, d.name
FROM &data as d(id int, name text) TYPE LIST;
```

### Updates
```sql
UPDATE Dictionary.users as usr
SET name = &fields.name
WHERE id = &fields.id::NUMBER;
```

### Structure Creation
```sql
CREATE DICTIONARY IF NOT EXISTS orders (
    id NUMERIC NOT NULL,
    user DICTIONARY.users FIELD,
    created TIMESTAMP DEFAULT now(),
    PRIMARY KEY (id)
);
```

Join the beta and get early access! 🔗 [[registration link](https://ledgyx.com/login)]

 🔗  [[Documentations link](https://docs.ledgyx.com/)]
