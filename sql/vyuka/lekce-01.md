## Lekce 1

## Data, databáze, Snowflake, tabulky, dotazy

- Co je to SQL a jak se používá k práci s databázemi?
- Připojení k databázi Snowflake a spouštění dotazů
  - Nastavení účtu Snowflake a přístup k webovému rozhraní
  - Spouštění dotazů a zobrazování výsledků ve webovém rozhraní
- Představení databáze `COURSES` ve Snowflake a její struktury
  - Schémata, role
  - Global Terrorism Database a její účel
  - Popis tabulky teror

## Základní syntaxe příkazu `SELECT`

#### `SELECT ... FROM ... WHERE ... ORDER BY ... LIMIT ...;`

- Získávání dat z jedné tabulky: `SELECT`, `FROM`, `WHERE`
- Výběr konkrétních sloupců: seznam názvů sloupců, zástupný znak (\*), `EXCLUDE`
- Třídění výsledků: `ORDER BY`, `ASC`, `DESC`
- Omezení výsledků: `LIMIT`
- Filtrování řádků pomocí klauzule `WHERE`
  - Porovnávací operátory: `=`, `>`, `>=`, `<`, `<=`, `<>`, `BETWEEN`, `IN`, `LIKE`, `IS NULL`
  - Logické operátory: `AND`, `OR`, `NOT`

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
  FROM TEROR;
  ```

  ::fig[První dotaz]{src=assets/Lesson01-01.png}

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
- Pokud chceme skoro všechny sloupečky, můžeme se nechtěných zbavit klauzulí `EXCLUDE` (snowflake, bigquery, teradata ...)
  ```sql
  SELECT * EXCLUDE (eventid, eventdade)
  ```
- Použití klauzule `FROM` k určení tabulky, ze které chcete vybírat data
  ```sql
  FROM TEROR
  ```
- Použití klauzule `WHERE` k filtrování řádků podle určitých podmínek
  ```sql
  WHERE iyear >= 2000
  ```
- Náš první "kompletní" dotaz, vybere tři sloupce a všechny řádky z tabulky teror
  ```sql
  SELECT eventid, iyear, country_txt
  FROM TEROR;
  ```
- Sloupečky můžeme přejmenovávat, jak se nám hodí (pomocí `AS`)
  ```sql
  SELECT eventid AS ID_UDALOSTI, iyear AS ROK, country_txt AS ZEME
  FROM TEROR;
  ```
- Sloupečky můžeme přejmenovávat a protože lenosti se meze nekladou, `AS` můžeme vynechat
  ```sql
  SELECT eventid ID_UDALOSTI, iyear ROK, country_txt ZEME
  FROM TEROR;
  ```
- V SQL je úplně jedno, kde máme mezery a konce řádek. Toho se dá využít k formátování kódu
  ```sql
  SELECT
         eventid     AS ID_UDALOSTI
      ,  iyear       AS ROK
      ,  country_txt AS ZEME
  FROM TEROR;
  ```
- Komentáře jsou zlaté drobečky, které nás zachrání, až se jednou ke kódu vrátíme, nebo se na něj bude dívqt někdo jiný a bude se snažit pochopit, co má všechno to SQL znamenat
  ```sql
  SELECT
         eventid     AS ID_UDALOSTI -- prejmenovani sloupecku pro ucely reportu
      ,  iyear       AS ROK         -- prejmenovani sloupecku pro ucely reportu
      ,  country_txt AS ZEME        -- prejmenovani sloupecku pro ucely reportu
  FROM TEROR;
  /*
  Z tabulky bereme vsechny radky a tri sloupecky:
     - eventid
     - iyear
     - country_txt
  Ty uz jsou davno vycistene a staci nam na vsechny reporty, ktere kdy kdo vymysli. Detaily se daji najit na http://detaily.com/terorism
  */
  ```
- Náš první dotaz, s filtrem
  ```sql
  SELECT eventid, iyear, country_txt
  FROM TEROR
  WHERE iyear >= 2019; -- jen udalosti od roku 2000
  ```

### Třídění výsledků: `ORDER BY`, `ASC`, `DESC`

- Seřadímě si výsledky
  ```sql
  SELECT eventid, iyear, country_txt
  FROM TEROR
  ORDER BY eventid;
  ```
- Seřadímě si výsledky opačně
  ```sql
  SELECT eventid, iyear, country_txt
  FROM TEROR
  ORDER BY eventid DESC;
  ```
- Seřadímě si výsledky podle více sloupců
  ```sql
  SELECT eventid, iyear, country_txt
  FROM TEROR
  ORDER BY iyear DESC, imonth DESC;
  ```

### Omezení výsledků: `LIMIT`

- Vybereme jen prvních 5 řádek
  ```sql
  SELECT eventid, iyear, country_txt
  FROM TEROR
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```
- Vybereme jen prvních 5000 řádek
  ```sql
  SELECT eventid, iyear, country_txt
  FROM TEROR
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5000;
  ```

### Filtrování řádků pomocí klauzule `WHERE`

#### Porovnávací operátory: `=`, `>`, `>=`, `<`, `<=`, `<>`, `BETWEEN`, `IN`, `LIKE`, `ILIKE`

- Filtrujeme podle zemí
  ```sql
  SELECT eventid, iyear, country_txt
  FROM TEROR
  WHERE country_txt = 'Afghanistan'
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```
- Filtrujeme podle zemí
  ```sql
  SELECT eventid, iyear, country_txt
  FROM TEROR
  WHERE country_txt in ('Austria', 'Germany')
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```
- Filtrujeme země začínající na A ...
  ```sql
  SELECT eventid, iyear, country_txt
  FROM TEROR
  WHERE country_txt like 'A%'
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```
- Filtrujeme země začínající na A/a ...
  ```sql
  SELECT eventid, iyear, country_txt
  FROM TEROR
  WHERE country_txt ilike 'A%'
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```
- Filtrujeme roky mezi ...
  ```sql
  SELECT eventid, iyear, country_txt
  FROM TEROR
  WHERE iyear BETWEEN 2018 AND 2019
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```

### Logické operátory: `AND`, `OR`, `NOT`

- `AND` znamená, že obě spojené podmínky musí platit zároveň `country_txt = 'Germany' AND weaptype1_txt ='Fake weapons'` vybere jen útoky fake zbraněmi v Německu
- `OR` znamená, že aspoň jedna ze spojených podmínek musí platit `country_txt = 'Germany' AND weaptype1_txt ='Fake weapons'` vybere všechny útoky fake zbraněmi a všechny útoky v Německu
- `NOT` podmínku logicky obrátí, `NOT iyear>2023` vybere všechny útoky roky menší nebo rovné 2013

- Kombinujeme filtry/podmínky ...

  ```sql
  SELECT eventid, iyear, country_txt
  FROM TEROR
  WHERE iyear BETWEEN 2018 AND 2019 AND country_txt ilike 'A%'
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```

  ```sql
  SELECT eventid, iyear, country_txt
  FROM TEROR
  WHERE iyear = 2000 OR iyear = 2015
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```

  ```sql
  SELECT eventid, iyear, country_txt
  FROM TEROR
  WHERE country_txt ilike 'A%' OR country_txt = 'Germany'
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```

- Negujeme podmínky

  ```sql
  SELECT eventid, iyear, country_txt
  FROM TEROR
  WHERE NOT country_txt ilike 'A%' OR country_txt = 'Germany'
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```

- Bacha na závorky

  ```sql
  SELECT eventid, iyear, country_txt
  FROM TEROR
  WHERE NOT (country_txt ilike 'A%' OR country_txt = 'Germany')
  ORDER BY iyear DESC, imonth DESC
  LIMIT 5;
  ```

### Unikátní hodnoty `DISTINCT`

Použitím operátoru `DISTINCT` získáme unikátní hodnoty z celého dotazu.

- seznam měst v tabulce

  ```sql
    SELECT DISTINCT CITY
    FROM TEROR;
    -- vrátí seznam měst v tabulce
  ```

- seznam kanadských měst v tabulce

  ```sql
    SELECT DISTINCT CITY
    FROM TEROR
    WHERE COUNTRY_TXT = 'Canada';
    -- vrátí seznam kanadských měst v tabulce
  ```

- sežazený seznam roků a měsíců, ve kterých v kanadě proběhl nějaký útok

  ```sql
    SELECT DISTINCT IYEAR, IMONTH
    FROM TEROR
    WHERE COUNTRY_TXT = 'Canada'
    ORCER BY IYEAR, IMONTH;
    -- vrátí sežazený seznam roků a měsíců, ve kterých v kanadě proběhl nějaký útok
  ```

### Počet řádek odpovídajících podmínce `COUNT(*)`

- počet všech řádek v tabulce
  ```sql
    SELECT COUNT(1)
    FROM TEROR;
    -- vrátí počet všech řádek v tabulce
  ```
- počet útoků (řádek) v Kanadě
  ```sql
    SELECT COUNT(*)
    FROM TEROR
    WHERE COUNTRY_TXT = 'Canada';
    -- vrátí počet útoků (řádek) v Kanadě
  ```
- počet útoků (řádek) v Kanadě v roce 2019

  ```sql
    SELECT COUNT(1)
    FROM TEROR
    WHERE COUNTRY_TXT = 'Canada' AND IYEAR = 2019
    ORCER BY IYEAR, IMONTH;
    -- vrátí počet útoků (řádek) v Kanadě v roce 2019
  ```

### A teď se podíváme, co na to robot, jak by nás to učil?

- vysvětlení základů docela trefí
  ::fig[Základy s chatGPT]{src=assets/Lesson01-chatGPT-01.png}

- vysvětlení konkrétní odrážky z lekce
  ::fig[SELECT, FROM, WHERE]{src=assets/Lesson01-chatGPT-02.jpg}

- a když nám to nestačí
  ::fig[Složitější příklady]{src=assets/Lesson01-chatGPT-03.png}

- a můžeme mu dát i větší kusy
  ::fig[Složitější příklady]{src=assets/Lesson01-chatGPT-04.png}

- nebo ho nechat shrnout celou lekci (screenshot je trochu zkrácený, lekce je celkem dlouhá)
  ::fig[Shrnutí lekce]{src=assets/Lesson01-chatGPT-05.png}

- Něco odpoví
  ::fig[Shrnutí lekce]{src=assets/Lesson01-chatGPT-06a.png}

- A když se nám to nelíbí, protě ho to necháme zkusit znova, napodruhé těžko říct, jestli lepší, ale jiná odpověď to určitě je
  ::fig[Shrnutí lekce]{src=assets/Lesson01-chatGPT-06b.png}
