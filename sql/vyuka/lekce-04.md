## Lekce 4 - Joiny, neboli spojování dat z víc tabulek

## Čemu se budeme věnovat?

Doteď jsme si hráli s tabulkou teror, která je trochu specifická. Je v ní všechno, co potřebujeme vědět Takovým tabulkám se říka supertables, nebo denormalizované. Dost často se ale data v databázi vyskytují v několika tabulkách a jsou spojené přes identifikátory.

V lekci se seznámíme s některými základními technikami spojování SQL, `LEFT JOIN`, `INNER JOIN` a dalšími JOINy. Uvidíme, jak spojovat tabulky pomocí různých typů sloupců, včetně číselných a řetězcových. Probereme také některá pokročilejší témata, jako je spojování na základě více sloupců a filtrování výsledků spojení pomocí klauzule WHERE. Let's do this!

#### Sekce na uklidnění

Joinů existuje spousta druhů, jsou tu vysvětlené, ale většina analytiky se dá dělat přes `LEFT JOIN` a `INNER JOIN` a na všechno ostatní se vykašlat. Použití `FULL OUTER JOIN` se člověku přihodí dvakrát za dekádu, z toho jednou je to na pohovoru ;)

### První pokus:

Začněme příkladem: v tabulce `ORDERS` máme sloupce `DATUM`, `PRODUCT_ID`, `UNITS`, `DISCOUNT_ID`, `PRICE`. Jak je vidět, o produktech a slevách se tady nic nedozvíme. Budeme muset použít tabulky `PRODUCTS` a `DISCOUNTS` a nějak z nich dostat, co nás zajímá.
| DATUM | PRODUCT_ID | UNITS | DISCOUNT_ID |
|------------|------------|--------|-------------||
| 2021-10-01 | 001 | 5 | 001A |
| 2021-10-02 | 002 | 3 | 002B |
| 2021-10-03 | 003 | 2 | 003C |
| 2021-10-06 | 001 | 3 | 006F |
| 2021-10-07 | 002 | 4 | 007G |
| 2021-10-08 | 003 | 6 | 008H |
| ... | ... | ... | ... |

`PRODUCTS` a `DISCOUNTS` vypadají takhle:

| PRODUCT_ID | PRODUCT_NAME    | CATEGORY       | UNIT_PRICE |
| ---------- | --------------- | -------------- | ---------- |
| 001        | Rýže Basmati    | Potraviny      | 25.00      |
| 002        | Palačinky       | Pečivo         | 45.50      |
| 003        | Černý čaj       | Nápoje         | 30.75      |
| 004        | Kukuřičný chléb | Pečivo         | 20.00      |
| 005        | Ovocný džus     | Nápoje         | 60.00      |
| 006        | Sýr Eidam       | Mléčné výrobky | 80.00      |
| ...        | ...             | ...            | ...        |

| DISCOUNT_ID | DISCOUNT_TYPE | DISCOUNT_AMOUNT |
| ----------- | ------------- | --------------- |
| 001A        | Percentage    | 0.10            |
| 002B        | Amount        | 5.00            |
| 003C        | Percentage    | 0.20            |
| 004D        | Amount        | 2.50            |
| 005E        | Percentage    | 0.15            |
| ...         | ...           | ...             |

No a abychom dostali report objednávek, budeme muset tabulky spojit. Můžeme chtít vidět třeba:

| DATUM      | PRODUCT_NAME | CATEGORY  | UNITS | PRICE |
| ---------- | ------------ | --------- | ----- | ----- |
| 2021-10-01 | Rýže Basmati | Potraviny | 5     | 50.00 |
| 2021-10-02 | Palačinky    | Pečivo    | 3     | 76.50 |
| 2021-10-03 | Černý čaj    | Nápoje    | 2     | 31.50 |
| ...        | ...          | ...       | ...   | ...   |

A tohle spojování tabulek si v téhle lekci vysvětlíme.

### Cože? Ta moje nefunguje.

Spojením tabulek se nemyslí, že by se jedna přilepila dolů pod druhou. To v SQL taky jde, ale dostaneme se k tomu později u `UNION` operátoru. Spojováním se tedy myslí spojování, kdy se k sobě tabulky lepí ze strany, k původním sloupečkům se doplňují odpovídající sloupečky z další tabulky. Odpovídající znamená, že vyhovují spojovací podmínce/pravidlu, např.: `ORDERS` obsahují sloupec `PRODUCT_ID` a podle něj můžeme tabulku spojit s `PRODUCTS`. Vím, může to být složité na pochopení, tak jsem sem dal všechna různá vysvětlení, každému pomáhá k pochopení něco jiného. Good luck!

### Linky na různá další vysvětlení JOINů

- [Vizuálně přes vénovy diagramy](https://towardsdatascience.com/visual-sql-joins-4e3899d9d46c)
- [Vizuálně se spoustou povídání, ale přes tabulky](https://learnsql.com/blog/sql-joins-types-explained/), nejdůležitější obrázky níže jsem sem hodil

### Přes motýlky

Přes motýlky bohužel joiny vysvětlit nejdou.

### Přes průnik množin a vénovy diagramy

Vím, množiny nejspu nejpopulárnější téma a data analytička není hard core matematička. No stress, není to takový peklo, jak to zní.
::fig[Množiny]{src=assets/joins-all.png}

### Přes tabulky

Joiny spojují tabulky a pracují s řadky a sloupci, takže se to na tabulkách s řádky a sloupci celkem dobře ukazuje.

Left join jednoduše vrátí všechny řádky z první tabulky a z druhé ty, které se povedlo napojit.
::fig[left]{src=assets/joins-left.png}

Inner join jednoduše vrátí jenom řádky které se povede mezi tabulkami propojit.
::fig[inner]{src=assets/joins-inner.png}

## Dost bylo vysvětlování

Pojďme si to zkusit.

#### Základní JOIN

V našídatabázi máme nejen tabulku teror, ale i tabulku `TEROR2`, která neobsahuje sloupečky `NĚCO_TXT` (např. `COUNTRY_TXT`), a tabulky, které obsahují tyto hodnoty a odpovídající identifikátory.

Jak ma to? Když v SQL dotazu použijeme dvě tabulky, je dobré poté u sloupečků uvádět i z jaké tabulky sloupec chceme. Abychom nevypisovali pořád celé názvy tabulek používají se skoro vždy aliasy. Podobně jako můžeme "prejmenovat" sloupeček, můžeme si aliasem "prejmenovat" tabulku.

```sql
-- oba dotazy dopadnou stejne
SELECT COUNTRY.NAME,
    TEROR2.NKILL,
    TEROR2.NKILLTER,
    TEROR2.GNAME,
    TEROR2.LATITUDE,
    TEROR2.LONGITUDE
FROM TEROR2
    LEFT JOIN COUNTRY ON TEROR2.COUNTRY = COUNTRY.ID;

-- tady uz pouzijeme aliasy
SELECT C.NAME,
    T2.NKILL,
    T2.NKILLTER,
    T2.GNAME,
    T2.LATITUDE,
    T2.LONGITUDE
FROM TEROR2 AS T2
    LEFT JOIN COUNTRY AS C ON T2.COUNTRY = C.ID;

-- a protože AS je nepovinné, většinou bude dotaz vypadat takhle
SELECT C.NAME,
    T2.NKILL,
    T2.NKILLTER,
    T2.GNAME,
    T2.LATITUDE,
    T2.LONGITUDE
FROM TEROR2 T2
    LEFT JOIN COUNTRY C ON T2.COUNTRY = C.ID;

-- a protoze datařům i datařkám často chybí fantazie, potkáme nejčastěji tohle:
SELECT B.NAME,
    A.NKILL,
    A.NKILLTER,
    A.GNAME,
    A.LATITUDE,
    A.LONGITUDE
FROM TEROR2 A
    LEFT JOIN COUNTRY B ON A.COUNTRY = B.ID;
-- proč je lepší se poslednímu způsobu vyhnout je asi celkem jasné
```

#### Spojování dvou tabulek je pro másla, mazáci jedou tři a víc ;)

```sql
SELECT C.NAME,
    ATYP.NAME,
    T2.GNAME,
    T2.NKILL
FROM TEROR2 AS T2
    LEFT JOIN COUNTRY AS C ON T2.COUNTRY = C.ID
    LEFT JOIN ATTACKTYPE AS ATYP ON T2.ATTACKTYPE1 = ATYP.ID;
```

#### Zatím šlo všechno jak po drátkách

Pokud je v datech všechno v pořádku, je joinování brnkačka, jakmile ho člověk jednou pochopí. Ale. Ale v datech není nikdy všechno v pořádku. Kdyby v naší původní tabulce `PRODUCTS` bylo víc řádků se stejným identifikátorem, začne nám je join množit, ke každé objednávce s takovým identifikátorem nám join vrátí tolik řádek, kolikrát je identifikátor v tabulce `PRODUCTS`.
Proto je super, když číselníky (jak se podobným tabulkám říká) mají identifikátor unikátní, neopakující se. Tabulka `COUNTRY_DIRTY_DATA` je příkladem zabordelené tabulky, kde identifikátor není unikátní.

```sql
-- výsledek má víc řádků než naše původní teror2 tabulka
SELECT DD.NAME,
    T2.NKILL
FROM TEROR2 AS T2
    JOIN COUNTRY_DIRTYDATA AS DD ON T2.COUNTRY = DD.ID;

-- to si můžeme ověřit i přes COUNT
SELECT COUNT(*)
FROM TEROR2;

SELECT COUNT(*)
FROM TEROR2 AS T2
    JOIN COUNTRY_DIRTYDATA AS DD ON T2.COUNTRY = DD.ID;
```

#### A co je teda rozdíl mezi `LEFT JOIN` a `INNER JOIN`

Jak je vysvětleno v ůvodu, druh JOINu ovlivní, které řádky se nám vrátí. `JOIN` v SQL znamená `INNER JOIN`, pokud chceme použít jiný typ, musíme to specifikovat. Příklady, příklady.

```sql
SELECT C.NAME AS COUNTRY,
    ATYP1.NAME AS ATTACK_TYPE1,
    ATYP2.NAME AS ATTACK_TYPE2,
    ATYP3.NAME AS ATTACK_TYPE3
FROM TEROR2 AS T2 --tady říkáme, že chceme i řádky, které se nanajdou v tabulce country
    LEFT JOIN COUNTRY AS C ON T2.COUNTRY = C.ID
    JOIN ATTACKTYPE AS ATYP1 -- INNER JOIN
    ON T2.ATTACKTYPE1 = ATYP1.ID
    JOIN ATTACKTYPE AS ATYP2 -- INNER JOIN
    ON T2.ATTACKTYPE2 = ATYP2.ID
    JOIN ATTACKTYPE AS ATYP3 -- INNER JOIN
    ON T2.ATTACKTYPE3 = ATYP3.ID; -- default pro JOIN je INNER JOIN (neni treba specifikovat), vrati pouze zaznamy, pro ktere najde v obou tabulkach shodu
-- spousta záznamů  v tabulce teror2 nemá definovaný attacktype1,2,3, takže nám budou chybět

-- vrátí  všechny záznamy z tabulky teror2 a snaží se k nim dopárovat attack type
SELECT C.NAME AS COUNTRY,
    ATYP1.NAME AS ATTACK_TYPE1,
    ATYP2.NAME AS ATTACK_TYPE2,
    ATYP3.NAME AS ATTACK_TYPE3
FROM TEROR2 AS T2
    LEFT JOIN COUNTRY AS C ON T2.COUNTRY = C.ID
    LEFT JOIN ATTACKTYPE AS ATYP1 ON T2.ATTACKTYPE1 = ATYP1.ID
    LEFT JOIN ATTACKTYPE AS ATYP2 ON T2.ATTACKTYPE2 = ATYP2.ID
    LEFT JOIN ATTACKTYPE AS ATYP3 ON T2.ATTACKTYPE3 = ATYP3.ID;
```

#### Join už umíme, co dál?

Joiny jde kombinovat se vším, co už umíme, a se vším, co se ještě naučíme. Pojďme se podívat, jak můžeme filtrovat přes `WHERE`.

```sql
SELECT GNAME,
    CITY,
    NWOUND
FROM TEROR2 AS T2
    JOIN COUNTRY AS C ON T2.COUNTRY = C.ID
WHERE C.NAME = 'Slovak Republic'
    AND YEAR(T2.IDATE) = 2016;
```

#### Slovníček, synonyma

| Termín                                                               | Popis                                                                                                                        |
| -------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| Identifikátor (primární klíč, cizí klíč, unikátní klíč, prostě klíč) | Ideálně celé číslo nebo hash unikátně identifikující položku v databázi                                                      |
| Denormalizovaná tabulka (super table)                                | Tabulka, ke které už pro většinu dotazů žádnou další nepotřebujeme                                                           |
| Normalizovaná tabulka, faktová tabulka, fact table                   | Tabulka, ve které jsou všechny opakující se hodnoty vyjádřeny cizím klíčem a vazbou na tabulku s těmito hodnotami (číselník) |
| Číselník (look-up table, dimensional table, dimenze)                 | Tabulka obsahující unikátní identifikátory a detaily k nim                                                                   |
| Hash                                                                 | unikátní identifikátor, může vypadat třeba takhle 3A4B77-8876EE-11C3D0                                                       |
