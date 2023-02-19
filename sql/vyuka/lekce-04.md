## Lekce 4 - Joiny, neboli spojování dat z víc tabulek

## Čemu se budeme věnovat?

Doteď jsme si hráli s tabulkou teror, která je trochu specifická. Je v ní všechno, co potřebujeme vědět Takovým tabulkám se říka supertables, nebo denormalizované. Dost často se ale data v databázi vyskytují v několika tabulkách a jsou spojené přes identifikátory.

Příklad: v tabulce `ORDERS` máme sloupce `DATUM`, `PRODUCT_ID`, `UNITS`, `DISCOUNT_ID`, `PRICE`. Jak je vidět, o produktech a slevách se tady nic nedozvíme. Budeme muset použít tabulky `PRODUCTS` a `DISCOUNTS` a nějak z nich dostat, co nás zajímá.
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

Při práci s daty uloženými ve více tabulkách je často nutné informace z těchto tabulek kombinovat, aby bylo možné získat přehled a odpovědět na otázky. Spojování SQL poskytuje způsob, jak toho dosáhnout, a umožňuje sloučit data ze dvou nebo více tabulek na základě společných hodnot.

Teď se seznámíme s některými základními technikami spojování SQL, `LEFT JOIN`, `INNER JOIN` a dalšími JOINy. Uvidíme, jak spojovat tabulky pomocí různých typů sloupců, včetně číselných a řetězcových. Probereme také některá pokročilejší témata, jako je spojování na základě více sloupců a filtrování výsledků spojení pomocí klauzule WHERE. Let's do this!

### Cože? Ta moje nefunguje.

Spojením tabulek se nemyslí, že by se jedna přilepila dolů pod druhou. To v SQL taky jde, ale dostaneme se k tomu později u `UNION` operátoru. Spojováním se tedy myslí spojování, kdy se k sobě tabulky lepí ze strany, k našim třem sloupečkům v tabulce

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
