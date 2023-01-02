## Data, databáze, Snowflake, tabulky, dotazy

- Co je to SQL a jak se používá k práci s databázemi?
- Připojení k databázi Snowflake a spouštění dotazů
  - Nastavení účtu Snowflake a přístup k webovému rozhraní
  - Spouštění dotazů a zobrazování výsledků ve webovém rozhraní
- Představení databáze COURSES ve Snowflake a její struktury
  - Schémata, role
  - Global Terrorism Database a její účel
  - Popis tabulky teror

## Základní syntaxe příkazu `SELECT` (`FROM`, `WHERE`)

- Získávání dat z jedné tabulky: SELECT, FROM, WHERE
- Výběr konkrétních sloupců: seznam názvů sloupců, zástupný znak (\*), exclude
- Třídění výsledků: ORDER BY, ASC, DESC
- Omezení výsledků: LIMIT
- Filtrování řádků pomocí klauzule WHERE
  - Porovnávací operátory: =, >, >=, <, <=, <>, BETWEEN, IN, LIKE, IS NULL
  - Logické operátory: AND, OR, NOT

## Jdeme na to!

### Než začneme

- Nejjednodušší SQL kód, prázdný příkaz zakončený středníkem
  ```sql
  ;
  ```
- Nejjednodušší SQL kód, který něco vrátí
  ```sql
  SELECT 1;
  ```

### Získávání dat z jedné tabulky: `SELECT`, `FROM`, `WHERE`

- Nejjednodušší kód, který pracuje s tabulkou
  ```sql
  SELECT *
  FROM teror;
  ```
- Použití příkazu `SELECT` k výběru sloupců, které chcete zobrazit ve výsledcích
  ```sql
  SELECT eventid, iyear, country_txt
  ```
- Citlivost na velká malá písmena, používání dvojitých (!) uvozovek
  ```sql
  SELECT "eventid", "IYEAR", country_txt
  ```
- Můžete také vybrat všechny sloupce pomocí zástupného znaku \*
  ```sql
  SELECT *
  ```
- Pokud chceme skoro všechny sloupečky, můžeme se nechtěných zbavit klauzulí exclude (snowflake, bigquery, teradata ...)
  ```sql
  SELECT * EXCLUDE (eventid, eventdatde)
  ```
- Použití klauzule `FROM` k určení tabulky, ze které chcete vybírat data
  ```sql
  FROM teror
  ```
- Použití klauzule `WHERE` k filtrování řádků podle určitých podmínek
  ```sql
  WHERE iyear >= 2000
  ```
- Náš první "kompletní" dotaz, vybere tři sloupce z tabulky teror
  ```sql
  SELECT eventid, iyear, country_txt
  FROM global_terrorism;
  ```
- Náš první dotaz, s filtrem
  ```sql
  SELECT eventid, iyear, country_txt
  FROM global_terrorism
  WHERE iyear >= 2000;
  ```

### Třídění výsledků: `ORDER BY`, `ASC`, `DESC`

- Seřadímě si výsledky
  ```sql
  SELECT eventid, iyear, country_txt
  FROM global_terrorism
  WHERE iyear >= 2000
  ORDER BY eventid;
  ```
- Seřadímě si výsledky opačně
  ```sql
  SELECT eventid, iyear, country_txt
  FROM global_terrorism
  WHERE iyear >= 2000
  ORDER BY eventid DESC;
  ```
- Seřadímě si výsledky podle více sloupců
  ```sql
  SELECT eventid, iyear, country_txt
  FROM global_terrorism
  WHERE iyear >= 2000
  ORDER BY iyear DESC, imonth DESC;
  ```

### Omezení výsledků: `LIMIT`

- Vybereme jen prvních 5 řádek
  ```sql
  SELECT eventid, iyear, country_txt
  FROM global_terrorism
  WHERE iyear >= 2000
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```
- Vybereme jen prvních 5000 řádek
  ```sql
  SELECT eventid, iyear, country_txt
  FROM global_terrorism
  WHERE iyear >= 2000
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5000;
  ```

### Filtrování řádků pomocí klauzule `WHERE`

#### Porovnávací operátory: `=`, `>`, `>=`, `<`, `<=`, `<>`, `BETWEEN`, `IN`, `LIKE`, `ILIKE`

- Filtrujeme podle zemí
  ```sql
  SELECT eventid, iyear, country_txt
  FROM global_terrorism
  WHERE country_txt = 'Afghanistan'
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```
- Filtrujeme podle zemí
  ```sql
  SELECT eventid, iyear, country_txt
  FROM global_terrorism
  WHERE country_txt in ('Austria', 'Germany')
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```
- Filtrujeme země začínající na A ...
  ```sql
  SELECT eventid, iyear, country_txt
  FROM global_terrorism
  WHERE country_txt like 'A%'
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```
- Filtrujeme země začínající na A/a ...
  ```sql
  SELECT eventid, iyear, country_txt
  FROM global_terrorism
  WHERE country_txt ilike 'A%'
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```
- Filtrujeme roky mezi ...
  ```sql
  SELECT eventid, iyear, country_txt
  FROM global_terrorism
  WHERE iyear BETWEEN 2000 AND 2015
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```

### Logické operátory: `AND`, `OR`, `NOT`

- Kombinujeme filtry/podmínky ...
  ```sql
  SELECT eventid, iyear, country_txt
  FROM global_terrorism
  WHERE iyear BETWEEN 2000 AND 2015 AND country_txt ilike 'A%'
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```
  ```sql
  SELECT eventid, iyear, country_txt
  FROM global_terrorism
  WHERE iyear = 2000 OR iyear = 2015
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```
  ```sql
  SELECT eventid, iyear, country_txt
  FROM global_terrorism
  WHERE country_txt ilike 'A%' OR country_txt = 'Germany'
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```
- Negujeme podmínky
  ```sql
  SELECT eventid, iyear, country_txt
  FROM global_terrorism
  WHERE not country_txt ilike 'A%' OR country_txt = 'Germany'
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```
- Bacha na závorky
  ```sql
  SELECT eventid, iyear, country_txt
  FROM global_terrorism
  WHERE not (country_txt ilike 'A%' OR country_txt = 'Germany')
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```
