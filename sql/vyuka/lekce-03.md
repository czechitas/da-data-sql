## Lekce 3 - Agregace dat, agregační funkce, `GROUP BY`

## Čemu se budeme věnovat?

#### [Agregační funkce](https://docs.snowflake.com/en/sql-reference/functions-aggregation):

- [SUM()](https://docs.snowflake.com/en/sql-reference/functions/sum.html): vypočítá součet hodnot v daném sloupci
- [COUNT()](https://docs.snowflake.com/en/sql-reference/functions/count.html): spočítá počet řádků v tabulce nebo počet hodnot v daném sloupci
- [AVG()](https://docs.snowflake.com/en/sql-reference/functions/avg.html): vypočítá průměrnou hodnotu v daném sloupci
- [MAX()](https://docs.snowflake.com/en/sql-reference/functions/min.html): najde maximální hodnotu v daném sloupci
- [MIN()](https://docs.snowflake.com/en/sql-reference/functions/min.html): najde minimální hodnotu v daném sloupci
- [GROUP BY](https://docs.snowflake.com/en/sql-reference/constructs/group.html): vytvoří skupiny podle hodnot v daném sloupci
- [HAVING](https://docs.snowflake.com/en/sql-reference/constructs/having.html): umožní filtrování výsledků agregovaných funkcí

#### Další funkce:

- [QUALIFY](https://docs.snowflake.com/en/sql-reference/constructs/qualify.html): umožňuje zapsat podmínky ke window funkcím
- [LISTAGG()](https://docs.snowflake.com/en/sql-reference/functions/listagg): sloučí více hodnot v daném sloupci do jednoho řetězce
- [COUNT(DISTINCT)](https://docs.snowflake.com/en/sql-reference/functions/count-distinct.html): spočítá počet unikátních hodnot v daném sloupci

#### `GROUP BY`

GROUP BY slouží k vytváření skupin z dat v tabulce. Pokud máme například tabulku teror s informacemi o teroristických útocích, můžeme pomocí GROUP BY vytvořit skupiny podle zemí, ve kterých se útoky odehrály. Takto zapsaný dotaz nám vypíše počet útoků v každé zemi:

```sql
SELECT country_txt, COUNT(*) as pocet_utoku
FROM teror
GROUP BY country_txt WHERE WEAPTYPE1_TXT = 1;
```

#### `HAVING`

HAVING slouží k filtrování výsledků agregovaných funkcí (jako jsou SUM(), AVG() nebo COUNT()). Používá se obvykle s GROUP BY a umožňuje nám získat pouze výsledky, které splňují určité podmínky. Například, pokud bychom chtěli získat pouze řádky s počtem útoků větším než 10, můžeme použít následující dotaz s HAVING:

```sql
SELECT region_txt, COUNT(*) as pocet_utoku
FROM teror
GROUP BY region_txt
HAVING COUNT(*) > 10;
```

#### `QUALIFY`

Použití QUALIFY umožňuje aplikovat filtr na okenní funkce. Podobně jako HAVING, umožňuje nám získat pouze ty řádky, které splňují určitou podmínku. Například, pokud bychom chtěli vypsat pouze řádky s nejvyšším počtem zraněných v každém roce, můžeme použít následující dotaz s QUALIFY:

```sql
SELECT iyear, nkill + nwound as pocet_zranenych,
       RANK() OVER (PARTITION BY iyear ORDER BY nkill + nwound DESC) as rank
FROM teror
QUALIFY rank = 1;
```

#### Jak jako `WHERE` a `HAVING` v čem je rozdíl?

WHERE a HAVING jsou klauzule používané k filtrování výsledků dotazu, ale liší se v tom, na co se používají.

Klausule WHERE se používá pro filtrování řádek na základě hodnot v jednotlivých sloupcích v tabulce. Slouží tedy k filtraci dat před jejich zpracováním v dotazu. Pokud máme například tabulku s útoky teroristů a chceme vypsat útoky, které se odehrály v USA, použijeme klauzuli WHERE takto:

```sql
SELECT eventid, iyear, country_txt
FROM teror
WHERE country_txt = 'United States';
```

HAVING se na druhé straně používá pro filtrování výsledků agregovaných funkcí (SUM, AVG, COUNT, atd.) používaných v GROUP BY. Slouží tedy k filtraci výsledků po jejich zpracování. Pokud chceme například vypočítat průměrný počet zraněných při útocích v každé zemi a vypsat pouze země s průměrným počtem zraněných větším než 100, použijeme klauzuli HAVING takto:

```sql
SELECT country_txt, AVG(nkill + nwound) as prumerny_pocet_zranenych
FROM teror
GROUP BY country_txt
HAVING AVG(nkill + nwound) > 100;

```

Takže WHERE se používá pro filtrování řádků vstupních dat, zatímco HAVING se používá pro filtrování výsledků dotazu.

### Příklady použití agregačních funkcí:

```sql
-- Výpočet součtu počtu zraněných a mrtvých osob při teroristických útocích v roce 2019
SELECT SUM(nkill + nwound) as celkovy_pocet_zranenych
FROM teror
WHERE iyear = 2019;

-- Spočítání počtu útoků v jednotlivých letech v Austrálii
SELECT iyear, COUNT(*) as pocet_utoku
FROM teror
WHERE country_txt = 'Australia'
GROUP BY iyear;

-- Vypočítání průměrného počtu obětí při teroristických útocích v Belgii
SELECT AVG(nkill + nwound) as prumerny_pocet_obeti
FROM teror
WHERE country_txt = 'Belgium';

-- Vyhledání největšího počtu obětí při teroristickém útoku v historii
SELECT MAX(nkill + nwound) as nejvyssi_pocet_obeti
FROM teror;

-- Najití nejmenšího počtu zraněných osob při teroristických útocích v roce 2018 v Německu
SELECT MIN(nwound) as nejnizsi_pocet_zranenych
FROM teror
WHERE iyear = 2018 AND country_txt = 'Germany';

-- Výpočet počtu teroristických útoků v jednotlivých městech v Indii
SELECT city, COUNT(*) as pocet_utoku
FROM teror
WHERE country_txt = 'India'
GROUP BY city;

-- Vypsání počtu útoků v jednotlivých zemích, které začínají na písmeno S
SELECT LEFT(country_txt, 1) as prvni_pismeno, COUNT(*) as pocet_utoku
FROM teror
WHERE country_txt LIKE 'S%'
GROUP BY prvni_pismeno;

-- Vypsání počtu unikátních let v datech teroristických útoků
SELECT COUNT(DISTINCT iyear) as pocet_let
FROM teror;

-- Vytvoření řetězce se jmény všech organizací, které provedly teroristické útoky v USA
SELECT GROUP_CONCAT(DISTINCT gname) as organizace_USA
FROM teror
WHERE country_txt = 'United States';

-- Vypsání počtu teroristických útoků v jednotlivých letech v USA, pokud měly více než 100 obětí
SELECT iyear, COUNT(*) as pocet_utoku
FROM teror
WHERE country_txt = 'United States' AND (nkill + nwound) > 100
GROUP BY iyear
HAVING COUNT(*) > 1;
```
