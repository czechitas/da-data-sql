## Lekce 7 - UNION a WINDOW funkce

## `UNION`, `UNION ALL`

#### Čemu se budeme věnovat?

Příkazy `UNION`, `UNION ALL` lze asi nejjedodušeji popsat jako vertikální lepení tabulek k sobě, respektive pod sebe.
Výsledkem je pak jedna tabulka.

Hlavní rozdíl mezi `UNION` a `UNION ALL` spočívá v tom, že `UNION` odstraňuje duplicitní řádky, zatímco `UNION ALL` je zachovává. To znamená, že pokud máte dvě tabulky se stejnými daty, ale některé řádky se vyskytují v obou tabulkách, `UNION` je odstraní, zatímco `UNION ALL` je zachová.

Použití `UNION` a `UNION ALL` závisí na konkrétní situaci. Pokud chcete sloučit výsledky dotazů a získat pouze unikátní řádky, použijte `UNION`. Pokud vám záleží na zachování všech řádků a chcete získat kompletní seznam výsledků, použijte `UNION ALL`.

##### Co je důležité

- stejná struktura = stejný počet sloupečků v jednotlivých dotazech, které spojujeme UNIONem
- stejný datový typ spojovaných sloupečků
- sloupečky v každém selectu/dotazu musí být ve stejném pořadí (bez ohledu na jméno sloupce)
- názvy sloupečků ve výsledku se berou z tabulky v prvním dotazu/selectu

##### Základní syntaxe

  ```sql
    SELECT weaptype1 
    FROM TEROR2
 
    UNION

    SELECT weaptype2
    FROM TEROR2
    ;
  ```

  Budeme například chtít seznam všech unikátních weaptypes, které byly použity při útocích. Zároveň si řekneme, že nechceme vidět jen id, ale chce vidět názvy. 
  Víme, že názvy weaptype jsou uloženy v tabulce `WEAPTYPE` a v tabulce `TEROR2` se weaptype vyskytuje ve třech sloupečcích, a proto použijeme `UNION`:

  ```sql
    SELECT w.NAME 
    FROM TEROR2 AS t
    JOIN WEAPTYPE AS w
    ON t.WEAPTYPE1 = w.ID

    UNION --> maže duplicity

    SELECT w.NAME
    FROM TEROR2 AS t
    JOIN WEAPTYPE AS w
    ON t.WEAPTYPE2 = w.ID

    UNION --> maže duplicity

    SELECT w.NAME
    FROM TEROR2 AS t
    JOIN WEAPTYPE AS w
    ON t.WEAPTYPE3 = w.ID
    ;
  ```

Dále nás například zajímá, jakým typem zbraně byl proveden útok, kolik to zasáhlo lidí a s jakým výsledkem (ranění, rukojmí, mrtví). Zároveň si vytvoříme sloupec `TYP_ZBRANE`, který nám bude říkat, o který typ zbraně šlo (weaptype1, 2 nebo 3)

```sql
 SELECT 
   w.NAME
  ,'WEAPTYPE1' AS typ_zbrane
  ,SUM(NKILL) AS mrtvi
  ,SUM(CASE WHEN NHOSTKID = -99 THEN NULL ELSE NHOSTKID END) AS rukojmi 
  -- nebo SUM(IFF(nhostkid=-99,NULL,NHOSTKID)) AS rukojmi
  ,SUM(NWOUND) AS raneni 
 FROM TEROR2 AS t
 JOIN WEAPTYPE AS w
 ON t.WEAPTYPE1 = w.ID
 GROUP BY w.NAME --místo w.NAME lze napsat 1

 UNION ALL

 SELECT 
   w.NAME
  ,'WEAPTYPE2'
  ,SUM(NKILL) AS mrtvi
  ,SUM(CASE WHEN NHOSTKID = -99 THEN NULL ELSE NHOSTKID END) AS rukojmi
  ,SUM(NWOUND) AS raneni
 FROM TEROR2 AS t
 JOIN WEAPTYPE AS w
 ON t.WEAPTYPE2 = w.ID
 GROUP BY w.NAME

 UNION ALL

  SELECT 
   w.NAME
  ,'WEAPTYPE3'
  ,SUM(NKILL) AS mrtvi
  ,SUM(CASE WHEN NHOSTKID = -99 THEN NULL ELSE NHOSTKID END) AS rukojmi
  ,SUM(NWOUND) AS raneni
 FROM TEROR2 AS t
 JOIN WEAPTYPE AS w
 ON t.WEAPTYPE3 = w.ID
 GROUP BY w.NAME
 ORDER BY NAME
;
```

## WINDOW funkce

#### Čemu se budeme věnovat?

Window funkce (můžete se setkat i s názvem analytické funkce) slouží k vytvoření nového sloupce výpočty prováděnými na určité podmnožině dat. 
Můžete tedy provádět výpočty nad vaším výběrem dat, aniž byste museli tuto část dat oddělit do samostatné tabulky nebo ji seskupit (pomocí GROUP BY).

Window funkce pracují nad záznamy vaší tabulky a umožňují vám definovat tzv. **"okna"** - tedy části dat, nad kterými chcete provést výpočet. 
Klauzule `OVER` a `PARTITION BY` společně definují to, co se nazývá jako "okno". Okno se dá dále omezit různými způsoby, například pomocí `ROWS BETWEEN` nebo `RANGE BETWEEN`, což určí, kolik řádků od aktuálního řádku se má do výpočtu zahrnout. To už jsme ale v pokročilejším použití.


Některé z nejčastěji používaných Window funkcí zahrnují `ROW_NUMBER`, `RANK` a `DENSE_RANK`, které se používají k přiřazení číselné hodnoty jednotlivým řádkům, na základě určitého řazení (tzv. *Rank-related functions*). 

A co tyhle tři vybrané funkce dělají?

*`ROW_NUMBER` - vrací unikátní číslo pro každý řádek v definované podskupině, čísluje od 1
*`RANK` - vrací pořadí (číslo) hodnoty v definované podskupině, čísluje od 1 a pokud jsou hodnoty stejné, mají stejné pořadí - následující číslo v pořadí se ale v tomto případě přeskočí
*`DENSE_RANK` - vrací pořadí (číslo) hodnoty v definované podskupině, čísluje od 1 a pokud jsou hodnoty stejné, mají stejné pořadí - následující číslo v pořadí se v tomto případě nepřeskakuje

Další Window funkce zahrnují `SUM`, `AVG` nebo `COUNT`, které se používají k výpočtu agregovaných hodnot nad daným oknem (tzv. *Window frame functions*).

Kompletní seznam Window funkcí ve Snowflaku můžete najít tady: https://docs.snowflake.com/en/sql-reference/functions-analytic


##### Základní syntaxe

```sql
 <function> ( [ <arguments> ] ) OVER ( [ PARTITION BY <expr1> ] [ ORDER BY <expr2> ] )
```
`OVER` indikuje začátek window funkce, což způsobí, že výsledky agregace budou přidány jako sloupec do výstupní tabulky bez vyžadování agregace pomocí GROUP BY.

`PARTITION BY` definuje, pokud existuje, dělení na podskupiny pro okno, tj. jak budou data seskupena před aplikací funkce (například podle města, roku apod.). Podklauzule PARTITION BY je volitelná. Lze analyzovat celou skupinu řádků, aniž bychom ji rozdělili na podskupiny.

`ORDER BY` řadí řádky v rámci okna. To se liší od řazení výstupu dotazu. Dotaz může mít jednu klauzuli ORDER BY, která ovládá pořadí řádků v rámci okna, a samostatnou klauzuli ORDER BY mimo klauzuli OVER, která ovládá pořadí výstupu celého dotazu.
**I když je klauzule ORDER BY volitelná pro některé Window funkce, je pro jiné povinná.** Vždy se tedy podívejte do dokumentace, pokud funkci ještě neznáte. 😉

### QUALIFY

Při používání Window funkcí nám ještě může pomoct klauzule `QUALIFY`, která umožňuje filtrovat výsledky Window funkcí. QUALIFY s Window funkcemi to, co HAVING dělá s GROUP BY (a agragačními funkcemi). Při zprocesování dotazu v databázi je QUALIFY vyhodnocována až poté, co jsou vyhodnoceny/vypočítány Window funkce. Pořadí vyhodnocení SELECTU je následující:

1. `FROM` + `JOIN`
2. `WHERE`
3. `GROUP BY`
4. `HAVING`
5. Window funkce
6. `QUALIFY`
7. `DISTINCT`
8. `ORDER BY`
9. `LIMIT`

❗ Nutno zmínit, že ne všechny databáze podporují klauzuli QUALIFY. Jedná se o rozšíření SQL, které podporují pouze některé databáze jako např. Snowflake nebo Teradata. V jiných databázích, jako je PostgreSQL, MySQL nebo MS SQL, je třeba použít poddotaz nebo CTE k dosažení podobné funkcionality. ❗

##### Co je důležité - shrnutí

Tři hlavní klíčová slova pro vytvoření Window funkce jsou:

- **OVER** - indikuje začátek Window funkce, definuje okno (společně s `PARTITION BY`)
- **PARTITION BY** - volitelné, umožňuje seskupovat řádky do podskupin.
- **ORDER BY** - určuje řazení řádků v okně.

- pro filtrování výsledku Window funkcí můžeme použít `QUALIFY` (v jiných databázích si ověřte v dokumentaci)

##### Příklady použití Window funkcí

Chceme seřadit organizace podle počtu obětí sestupně a přiřadit jim pořadí (rank).

```sql
 SELECT GNAME, SUM(NKILL) AS pocet_mrtvych
 ,RANK() OVER (ORDER BY SUM(NKILL) DESC) AS rank
 FROM TEROR
 WHERE NKILL IS NOT NULL
 GROUP BY GNAME
 ORDER BY SUM(NKILL) DESC;
```


Chceme seřadit organizace podle počtu obětí sestupně a přiřadit jim pořadí (rank) v rámci roku.

 ```sql
 SELECT GNAME, IYEAR, SUM(NKILL) AS pocet_mrtvych
 ,ROW_NUMBER() OVER (ORDER BY SUM(NKILL) DESC) AS rn
 ,RANK() OVER (PARTITION BY IYEAR ORDER BY SUM(NKILL) DESC) AS rank
 FROM TEROR
 WHERE NKILL IS NOT NULL
 GROUP BY GNAME, IYEAR
 ORDER BY SUM(NKILL) DESC;
```

Chceme seřadit organizace podle počtu obětí sestupně a přiřadit jim pořadí (rank) v rámci roku. Nakonec chceme vybrat jen první tři z každého roku.
 ```sql
 SELECT * FROM
 (SELECT GNAME, IYEAR, SUM(NKILL) AS pocet_mrtvych
 ,ROW_NUMBER() OVER (ORDER BY SUM(NKILL) DESC) AS rn
 ,RANK() OVER (PARTITION BY IYEAR ORDER BY SUM(NKILL) DESC) AS rank
 FROM TEROR
 WHERE NKILL IS NOT NULL
 GROUP BY GNAME, IYEAR
 ORDER BY rank, IYEAR DESC
 ) 
 WHERE rank <= 3;
```

A ještě si ukážeme, jak vyřešit stejné zadání, ale bez subselectu - budeme tedy přímo filtrovat pomocí `QUALIFY`.

 ```sql
SELECT 
      GNAME
      ,IYEAR
      ,SUM(NKILL) AS pocet_mrtvych
      ,RANK() OVER (PARTITION BY IYEAR
                    ORDER BY SUM(NKILL) DESC) AS rank
FROM TEROR
WHERE NKILL IS NOT NULL
GROUP BY GNAME,IYEAR
QUALIFY rank <= 3
ORDER BY rank, IYEAR DESC;
 ```