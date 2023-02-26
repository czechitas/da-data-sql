## Lekce 6 - Vnořené SELECTy a CTE

#### Čemu se budeme věnovat?

Vnořené selecty a CTE (Common Table Expressions) jsou dvě techniky, které umožňují psát složitější dotazy.

Vnořené selecty jsou selecty uvnitř jiného SELECTu. Tyto selecty se používají k výpočtu nebo filtrování dat a mohou být použity v různých částech dotazu, jako jsou `WHERE`, `HAVING` nebo `SELECT`. Vnořené selecty se používají v případech, kdy nejsou k dispozici vhodné agregační funkce nebo kdy je nutné použít poddotazy.

CTE fungují podobně, můžeš si je představit jako pojmenovaný vnořený select. Slouží k členění složitých dotazů na menší části a zlepšují čitelnost a údržbu dotazu.. CTE jsou definovány jako běžné SELECT dotazy, ale jejich výstup není uložen v databázi. Místo toho se používají v dalších částech dotazu.

Zní to složitě, ale postupně se k tomu dopracujeme.

- Uděláme si jednoduchý SELECT, který vrátí jedničku

  ```sql
  SELECT 1 AS A
  ```

| A   |
| --- |
| 1   |

- A teď ho "vnoříme"

  ```sql
  -- Vybere 1 jako subselect
  SELECT VNORENY.A
  FROM (
          SELECT 1 AS A
      ) AS VNORENY;
  ```

| A   |
| --- |
| 1   |

- Totéž, ale použijeme dotaz do tabulky

  ```sql
  -- Vybere unikátní dvojice skupiny a země jako subselect
  SELECT VNORENY.SKUPINA, VNORENY.ZEME
  FROM (
          SELECT DISTINCT T.GNAME AS SKUPINA,
              C.NAME AS ZEME
          FROM TEROR2 AS T
              INNER JOIN COUNTRY AS C ON T.COUNTRY = C.ID
      ) AS VNORENY;


  -- Jde to i bez aliasů
  SELECT SKUPINA, ZEME
  FROM (
          SELECT DISTINCT T.GNAME AS SKUPINA,
              C.NAME AS ZEME
          FROM TEROR2 AS T
              INNER JOIN COUNTRY AS C ON T.COUNTRY = C.ID
      ) AS VNORENY;
  ```

- A tenhle dozaz už bychom bez vnoření vymýšleli těžko

#### Zobrazení všech teroristických událostí, které spáchala teroristická organizace s největším počtem obětí

```sql
-- Zobrazení všech teroristických událostí, které spáchala teroristická
-- organizace s největším počtem obětí
SELECT GNAME,
    IYEAR,
    NKILL
FROM TEROR
WHERE GNAME =
    (
        SELECT GNAME
        FROM TEROR
        ORDER BY NKILL DESC
        LIMIT 1
    );
```

- A zesložiťování jde stupňovat

  #### Počet mrtvých v letech 2017 a 2016, které má na svědomí Islámský Stát tak, aby ve výsledku byl název organizace a ve sloupcích počet mrtvých dle let

- Vytvoříme si nejdřív dotaz na počty mrtvých v roce 2017

  ```sql
  SELECT GNAME,
    SUM(NKILL) AS POCETMRTV2017
  FROM TEROR
  WHERE IYEAR = 2017
    AND GNAME ILIKE '%islamic state%'
  GROUP BY 1
  ORDER BY POCETMRTV2017 DESC
  ```

- teď rok 2016

  ```sql
  SELECT GNAME,
    SUM(NKILL) AS POCETMRTV2017
  FROM TEROR
  WHERE IYEAR = 2016
    AND GNAME ILIKE '%islamic state%'
  GROUP BY 1
  ORDER BY POCETMRTV2017 DESC
  ```

- A spojíme je podle jména organizace ()

  ```sql
    -- Počet mrtvých v letech 2017 a 2016 které má na svědomí Islámský Stát tak,
    -- aby ve výsledku byl název organizace a ve sloupcích počet mrtvých dle let
    SELECT T1.*,
            T2.POCETMRTV2016
    FROM
    (
    SELECT GNAME, SUM(NKILL) AS POCETMRTV2017
    FROM TEROR
    WHERE IYEAR=2017 AND GNAME ILIKE '%islamic state%'
    GROUP BY 1
    ORDER BY POCETMRTV2017 DESC
      ) AS T1
    LEFT JOIN
    (
    SELECT GNAME, COUNT(NKILL) AS POCETMRTV2016
    FROM TEROR
    WHERE IYEAR=2016
    GROUP BY 1
    ORDER BY POCETMRTV2016 DESC
      ) AS T2
      ON T1.GNAME=T2.GNAME;
  ```

#### Výběr teoristických úroků v roce 2016, které má na svědomí Islámský Stát a doplnění informace max a min počtu oětí v roce 2016 ke každému útoku

```sql
SELECT T1.EVENTID, T1.GNAME, T1.IYEAR, T1.NKILL,
        T2.MAXMRTVYCH2016, T2.MINMRTVYCH2016
FROM TEROR AS T1
LEFT JOIN
(
SELECT GNAME, MAX(NKILL) AS MAXMRTVYCH2016, MIN(NKILL) AS MINMRTVYCH2016
FROM TEROR
WHERE IYEAR=2016 AND GNAME ILIKE '%islamic state%'
GROUP BY 1
  ) AS T2
ON T1.GNAME=T2.GNAME
WHERE T1.GNAME ILIKE '%islamic state%' AND T1.IYEAR=2016;
```

#### Dost vnořených SELECTů, pojďme na CTE

Rozdíl je vlastně jen v tom, že CTE pojmenujeme a definujeme na začátku dotazu a pak se používá jakoby to byla normální tabulka.

```sql
WITH CTEPODDOTAZ AS (
    SELECT 1 JEDNICKA,
        'MILION' DVOJKA
)
SELECT C.JEDNICKA,
    C.DVOJKA
FROM CTEPODDOTAZ C;
```

- Podobně i u složitějších dotazů

  ```sql
    WITH TERORCOUNTRY AS (
        SELECT DISTINCT T.GNAME SKUPINA,
            C.NAME ZEME
        FROM TEROR2 T
            INNER JOIN COUNTRY C ON T.COUNTRY = C.ID
    )
    SELECT *
    FROM TERORCOUNTRY;
  ```

- Porovnejte po letech počty rukojmích u útoků fake zbraněmi a normálními zbraněmi

  ```sql
  WITH RUKOJMI_PO_LETECH_FAKE AS (
    SELECT IYEAR,
        SUM(NHOSTKID) AS RUKOJMI_FAKE
    FROM TEROR
    WHERE WEAPTYPE1_TXT = 'Fake Weapons'
        AND NHOSTKID <> -99
    GROUP BY IYEAR
  ),
  RUKOJMI_PO_LETECH_BEZ_FAKE AS (
    SELECT IYEAR,
        SUM(NHOSTKID) AS RUKOJMI_BEZ_FAKE
    FROM TEROR
    WHERE WEAPTYPE1_TXT <> 'Fake Weapons'
        AND NHOSTKID <> -99
    GROUP BY IYEAR
  ) --SPOJENI PRES ROKY
  SELECT F.IYEAR,
    F.RUKOJMI_FAKE,
    BF.RUKOJMI_BEZ_FAKE
  FROM RUKOJMI_PO_LETECH_FAKE F
    LEFT JOIN RUKOJMI_PO_LETECH_BEZ_FAKE BF ON F.IYEAR = BF.IYEAR
  ORDER BY IYEAR;
  ```

| IYEAR | RUKOJMI_FAKE | RUKOJMI_BEZ_FAKE |
| ----- | ------------ | ---------------- |
| 1994  | 164          | 3270             |
| 1995  | 298          | 3646             |
| 1996  | 589          | 4658             |
| 1997  | 80           | 1449             |
| 2000  | 105          | 3315             |
| 2003  | 2            | 1248             |
| 2010  | 105          | 2248             |
| 2014  | 1            | 16216            |
| 2017  | 3            | 9560             |
| ...   | ...          | ...              |
