## Lekce 2 - Operátory a funkce

Operátory a funkce nám umožňují vytvářet sloupečky a filtrovat data podle určitého kritéria. Funkcí existuje mraky, v každé databázi najdeme minimálně ty základní. Ani operátory nebudou ve včech databázích stejné, ale teď se soustřeďme se na náš snowflake.

#### Základní operátory

- je větší `>`
- je menší `<`
- je menší nebo rovno `<=`
- je větší nebo rovno `>=`
- je není rovno `!=`, `<>`
- je rovno `=`

#### Práce s textem

- [SPLIT](https://docs.snowflake.com/en/sql-reference/functions/split.html) - Rozdělí řetězec na položky
- [SUBSTRING](https://docs.snowflake.com/en/sql-reference/functions/substr.html) - Vrátí část řetězce
- [LEFT](https://docs.snowflake.com/en/sql-reference/functions/left.html) - Vrátí část řetězce od začátku
- [RIGHT](https://docs.snowflake.com/en/sql-reference/functions/right.html) - Vrátí část řetězce od konce
- [UPPER](https://docs.snowflake.com/en/sql-reference/functions/upper.html) - Převede řetězec na velká písmena
- [LENGHT](https://docs.snowflake.com/en/sql-reference/functions/length.html) - Vrátí délku řetězce

#### Matematické funkce

- [HAVERSINE](https://docs.snowflake.com/en/sql-reference/functions/haversine.html) - Výpočet vzdálenosti
- [ROUND](https://docs.snowflake.com/en/sql-reference/functions/round.html) - Zaokrouhlení čísla
- [FLOOR](https://docs.snowflake.com/en/sql-reference/functions/floor.html) - Nejmenší celé číslo
- [CEIL](https://docs.snowflake.com/en/sql-reference/functions/ceil.html) - Největší celé číslo

#### Práce s datumem a časem

- [TO_DATE](https://docs.snowflake.com/en/sql-reference/functions/to_date.html) - Převod na datum
- [DATE_FROM_PARTS](https://docs.snowflake.com/en/sql-reference/functions/date_from_parts.html) - Sestavení data z částí
- [DATEADD](https://docs.snowflake.com/en/sql-reference/functions/dateadd.html) - Přidání času k datu
- [EXTRACT](https://docs.snowflake.com/en/sql-reference/functions/extract.html) - Výběr části data

#### Logické operátory `AND`, `OR`, `NOT`

Logické operátory asi lepší vysvětlit na příkladech.

```sql
-- použití operátoru AND
SELECT LEN('Prague')=5 AND 'Prague' LIKE 'G%';
-- výsledek: FALSE

-- použití operátoru OR
SELECT (LEN('Prague')=3) OR ('Prague' LIKE 'G%');
-- výsledek: TRUE

-- použití operátoru NOT
SELECT NOT (LEN('Prague')=3);
-- výsledek: TRUE

-- kombinace operátorů AND, OR a NOT
SELECT (LEN('Prague')=5) OR (NOT ('Prague' LIKE 'P%'));
-- výsledek: FALSE

-- využití závorek
SELECT ((LEN('Prague')=5) OR (LEN('Prague')<3)) AND ('Prague' LIKE 'P%');
-- výsledek: TRUE
```

Samotné hodnoty `TRUE` a `FALSE` nám moc k ničemu nejsou (i když neříkej hop), ale skvěle se nám hodí v podmínkách.

```sql
-- použití operátoru AND v podmínce WHERE
SELECT *
FROM TEROR
WHERE (LENGTH(CITY)=5) AND (CITY LIKE 'P%');
-- výsledek: vrátí řádky, kde délka hodnoty v sloupečku CITY
-- je rovna 5 a hodnota v sloupečku CITY začíná na 'P'

-- použití operátoru OR
SELECT *
FROM TEROR
WHERE (LENGTH(CITY)=3) OR (CITY LIKE 'P%');
-- výsledek: vrátí řádky, kde délka hodnoty v sloupečku CITY
-- je rovna 3 nebo hodnota v sloupečku CITY začíná na 'P'

-- použití operátoru NOT
SELECT *
FROM TEROR
WHERE NOT (LENGTH(CITY)=3);
-- výsledek: vrátí řádky, kde délka hodnoty v sloupečku CITY
-- není rovna 3

-- kombinace operátorů AND, OR a NOT
SELECT *
FROM TEROR
WHERE (LENGTH(CITY)=5) OR (NOT (CITY LIKE 'P%'));
-- výsledek: vrátí řádky, kde délka hodnoty v sloupečku CITY
-- je rovna 5 nebo hodnota v sloupečku CITY nezačíná na 'P'

-- využití závorek v podmínce WHERE
SELECT *
FROM TEROR
WHERE ((LENGTH(CITY)=5) OR (LENGTH(CITY)<3)) AND (CITY LIKE 'P%');
-- výsledek: vrátí řádky, kde délka hodnoty v sloupečku CITY
-- je buď rovna 5 nebo menší než 3 a hodnota v sloupečku CITY začíná na 'P'
```

#### BETWEEN

```sql
SELECT *
FROM TEROR
WHERE YEAR BETWEEN 1979 AND 2021;

-- Očekávaný výsledek:
-- Všechny řádky s rokem mezi 1979 a 2021
```

#### CASE

```sql
SELECT CITY,
CASE
  WHEN YEAR >= 1979 AND YEAR <= 2000 THEN 'Old Era'
  WHEN YEAR > 2000 THEN 'New Era'
  ELSE 'Before 1979'
END AS ERA
FROM TEROR;

-- Očekávaný výsledek:
-- Sloupce CITY a nový sloupec ERA, kde je podle roku události označení era
-- jako Old Era, New Era nebo Before 1979.
```

#### IFNULL (ajaj NULL, to lektor vysvětlí)

```sql
SELECT CITY,
  IFNULL(CITY, 'Unknown') AS CITY_NONULLS,
  IFNULL(RANSOMNOTE, 'Unknown') AS RANSOMNOTE_NONULLS
FROM TEROR
WHERE CITY is NULL or CITY = 'Unknown' order by CITY NULLS FIRST LIMIT 500; -- podmínka, která ukáže zajímavé řádky

-- Očekávaný výsledek:
-- Výstup bude obsahovat sloupečky CITY, CITY_NONULLS (kde se NULL hodnoty změní na "Unknown")
-- a RANSOMNOTE_NONULLS (kde se NULL hodnoty v RANSOMNOTE změní na "Unknown").
```

::fig[Shrnutí lekce]{src=assets/Lesson02-null.png}
