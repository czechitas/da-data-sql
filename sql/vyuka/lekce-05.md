## Lekce 5 - Vytváření a úpravy tabulek, import dat

## `INSERT`, `DELETE`, `CREATE`, `ALTER`

#### Čemu se budeme věnovat?

Příkazy pro manipulaci s daty v databázi. Vytváření tabulek, změny sloupců, vkládání, úpravy a mazání dat. Čeká na nás i kouzelný příkaz "at" pro výběr dat v určitém časovém okamžiku a pravděpodobně trochu bolavý import dat ze souboru (CSV).

Zatímco `SELECT`, kterým jsme se zabývali doteď, je dotazovací příkaz, který slouží k získání dat z databáze, ostatní příkazy se používají k úpravě dat v tabulkách.

Příkaz [`CREATE TABLE`](https://docs.snowflake.com/en/sql-reference/sql/create-table) slouží k vytvoření nové tabulky, zatímco [`ALTER TABLE`](https://docs.snowflake.com/en/sql-reference/sql/alter-table) slouží k úpravě struktury existující tabulky, například k přidání nebo odebrání sloupců.

Příkaz [`INSERT`](https://docs.snowflake.com/en/sql-reference/sql/insert) a `INSERT INTO` slouží k přidání nových řádek do existující tabulky, a to buď přímým vložením hodnot, nebo výběrem a vložením dat z jiné tabulky. [`UPDATE`](https://docs.snowflake.com/en/sql-reference/sql/update) slouží k úpravě stávajících dat v tabulce, zatímco [`DELETE`](https://docs.snowflake.com/en/sql-reference/sql/delete) slouží k odstranění dat z tabulky.

[`DROP TABLE`](https://docs.snowflake.com/en/sql-reference/sql/drop-table) slouží k odstranění celé tabulky z databáze.

Souhrnně lze říci, že zatímco SELECT slouží k dotazování a načítání dat z tabulky, ostatní příkazy slouží k úpravě nebo odstranění dat z tabulky, případně k vytvoření nebo úplnému odstranění tabulek.

Školometsky se příkazy dělí na `DML` a `DDL` Data Definition Language (`CREATE`, `ALTER`, `DROP`,`TRUNCATE`) a Data Manipulation Language (`INSERT`, `UPDATE`, `DELETE`). `SELECT` patří do skupiny DQL – Data Query Language.

#### Odkazy na soubory ke stažení

Tyhle soubory budou potřeba, až budeme importovat data.

- [Stáhnout DATA.CSV](assets/data.csv)
- [Stáhnout UKOL.CSV](assets/ukol.csv)

#### Nastavení pracovního schématu:

- Příkazy `USE DATABASE`; `USE SCHEMA`
- Naklikáním v UI
  Kontext databáze a schématu nastavujeme pro všechny pro následující příkazy. V tomto případě je aktuální databáze COURSES a aktuální schéma je nejprve nastaveno na SCH_CZECHITA a poté na <SCH_CZECHITA_PRIJIMENIK>.

  ```sql USE DATABASE COURSES;
  USE SCHEMA SCH_CZECHITA;
  USE SCHEMA <SCH_CZECHITA_PRIJIMENIK>;
  ```

#### Vytvoření naší první tabulky

- Let's go

  ```sql
  CREATE TABLE NEW_TEROR (
    ID INT,
    GNAME VARCHAR(250),
    NKILL INT,
    NWOUND INT
  );
  ```

- `CREATE TABLE` ⬆️⬆️ vytvoří tabulku s názvem NEW_TEROR se čtyřmi sloupci. U každého sloupce defimuje [datový typ](https://docs.snowflake.com/en/sql-reference/intro-summary-data-types), řekněme, že pro teď nám stačí vědět, že sloupeček může obsahovat, text/string, číslo nebo datum.

  ```sql
  CREATE TABLE NEW_TEROR (
    ID INT,
    GNAME VARCHAR(250),
    NKILL INT,
    NWOUND INT
  );
  ```

- Když pustíme `CREATE TABLE` znova, bude si stěžovat, že tabulka už existuje, že ji nemůže vytvořit.

  ```sql
  CREATE OR REPLACE TABLE NEW_TEROR (
        ID INT AUTOINCREMENT NOT NULL UNIQUE,
        GNAME VARCHAR(250),
        NKILL INT,
        NWOUND INT,
        CONSTRAINT ID_PK PRIMARY KEY (ID)
    );
  ```

- Ale on může, stačí mu říct, ať jí přepíše `CREATE OR REPLACE TABLE`, pokud tam je (a tím z ní nejdřív defacto smaže a pak ji vytvoří znova prázdnou).
- `AUTOINCREMENT NOT NULL UNIQUE`, heh? Každá databáze umí i víc než jen obyčejné datové typy, tady říkáme, že každý nový řádek bude mít ve sloupci `ID` číslo o jedna větší než řádek předchozí a že se nám nepovede vložit řádek s `ID`, které už v tabulce je (SNOWFLAKE nám to tedy dovolí, ale to je jiný příběh).

  ```sql
  CREATE TABLE UDALOSTI_JEN_V_CESKU AS
  SELECT GNAME,
    CITY,
    NKILL,
    NWOUND
  FROM SCH_CZECHITA.TEROR
  WHERE COUNTRY_TXT = 'Czech Republic';
  ```

- Tabulku lze vytvořit rovnou naplněnou daty. Stačí použít `CREATE TABLE [TABLE_NAME] AS SELECT`, zkráceně `CTAS`.

  ```sql
  CREATE TEMPORARY TABLE ORGANIZACE_PO_ZEMICH AS --docasna tabulka zanikne, kdyz se odhlasime
  SELECT GNAME,
    CITY,
    SUM (NKILL) KILLED,
    SUM (NWOUND) WOUNDED,
    C.NAME COUNTRYNAME
  FROM SCH_CZECHITA.TEROR2 T2
    LEFT JOIN SCH_CZECHITA.COUNTRY C ON C.ID = T2.COUNTRY
  GROUP BY C.NAME,
    T2.GNAME,
    T2.CITY;
  ```

- V každé databázi existuje něco jako dočasné tabulky, říká se jim `TEMPORARY`, `TEMP`, `EPHEMERAL` atd. Ve SNOWFLAKE `TEMPORARY`. Taková tabulka zanikne, když se od serveru odhlásíme, nebo když skončí naše session.

  ```sql
  ALTER TABLE NEW_TEROR
  ALTER COLUMN GNAME VARCHAR(350);

  ALTER TABLE NEW_TEROR
  ALTER COLUMN GNAME NOT NULL;

  --COLUMN je nepovinné
  ALTER TABLE NEW_TEROR
  ADD CONTINENT VARCHAR(300);

  ALTER TABLE NEW_TEROR DROP COLUMN CONTINENT;
  ```

- přidávání a odebírání sloupečlů je triviální.

  ```sql
  INSERT INTO NEW_TEROR (GNAME, NKILL)
  --v INSERTu DYCKY vyjmenovat sloupce, NIKDY nepoužívat hvězdičku
  SELECT GNAME,
    NKILL
  FROM TEROR
  WHERE IYEAR = 2015;
  ```

- Vkládáme do tabulky NEW_TEROR (GNAME, NKILL) všechny řádky z TEROR. Sloupeček NWOUND bude u všch těchto řádků prázdný. Tak jednoduchý to je.

  ```sql
  INSERT INTO NEW_TEROR (GNAME, NKILL, NWOUND)
  VALUES
    ('Žoldáci', 10, 1),
    ('Nosiči smrti', 15, 2),
    ('Nějácí další teroristi', 155, 5);
  ```

- Chceme si tabulku naplnit napevno nějakými daty? Brnkačka. Občas některé databáze dělají export dat právě v tomto formátu, jako dlouhatánskou nudli insert into a hodnot. Pak se celý ten skript vezme a spustí a voila, data jsou uvnitř. Není to nejlepší způsob, ale jde to.

  ```sql
  UPDATE NEW_TEROR
  SET NKILL = 0
  WHERE NKILL IS NULL;
  UPDATE NEW_TEROR
  SET NKILL = 0;
  -- pozor, UPDATE bez podmínky WHERE nastaví všude 0
  UPDATE NEW_TEROR
  SET NKILL = 100,
      NWOUND = 100
  WHERE GNAME = 'Žoldáci';
  -- lze updatovat i víc sloupců najednou
  ```

- `UPDATE`, mocný nástroj na úpravu dat v tabulce, čištění atd. Vždycky pozor na podmínku, pokud nechceme přepsat hodnotu na všech řádcích tabulky.

- `DELETE` jednoduše maže řádky. Stejně jako u UPDATE - pozor na `WHERE` podmínku.

  ```sql
  DELETE FROM NEW_TEROR
  WHERE NKILL is NULL;

  DELETE FROM NEW_TEROR;
  -- vymaže pouze data, všechny řádky v tabulce

  ```

- `DROP` AKA ultimátní řešení každého problému
- `DROP TABLE`
- `DROP SCHEMA`
- `DROP DATABASE`

  ```sql
  DROP TABLE NEW_TEROR;
  DROP DATABASE <jmeno>;
  ```

#### Příklad jako z pohádky

- Teď si zkusme všechno pokašlat. Děsnej den, na co člověk sáhne, to se poj..e. Asi takhle:

  - vytvoříme tabulku a naplníme, ji, zatím nám jsou bohové jedniček a nul nakloněni

    ```sql
    CREATE TABLE XX_PRYCSEMNOU AS
    SELECT GNAME,
        CITY,
        SUM (NKILL) KILLED,
        SUM (NWOUND) WOUNDED
    FROM TEROR
    WHERE IYEAR = 2016
    GROUP BY GNAME,
        CITY;
    ```

  - Ale v každý pohádce je zlej černokněžník, čarodějnice, nebo dokonce rovnou démon. Nejlepší je, že my si tabulku rozbijeme sami:

    ```sql
    UPDATE XX_PRYCSEMNOU
    SET KILLED = 0;
    -- tady nám trochu chybí podmínka, takže si přepíšeme všechny řádky
    -- chtěli jsme přepsat jen ty, kde jsou použité fake weapons, ajaj
    -- blbej den, no
    ```

  - Takhle nějak ta pohroma vypadá:

    ```sql
    SELECT *
    FROM XX_PRYCSEMNOU;
    ```

  - Kdybychom tak mohli vrátit čas třeba o 2 minuty zpátky. A to my můžeme, popelky střevíček se našel!
    ```sql
    SELECT *
    FROM XX_PRYCSEMNOU AT (OFFSET => -120);
    ```
  - [`AT (OFFSET ..)`](https://docs.snowflake.com/en/sql-reference/constructs/at-before) nás totiž umí posouvat v čase.

#### Import dat "přímo" do Snowflake

Dobrá rada nad zlato, pokud můžeš, použij na import dat do databáze nějaký nástroj. Já samozřejmě doporučuju Keboolu, ale existují další - Fivetran, Matillion, Azure data factory, Talend, Informatica. Je jich milión. Ale Snowflake umí naimportovat data i napřímo.

- Vytvoříme si tabulku, do které budeme importovat, bez toho to nepůjde. Ptáte se, jak máte znát její strukturu? Dobrá otázka. Proto jsem doporučoval tooly, ale drsňáci strukturu tabulky znají. Takhle vypadá tabulka v souboru [data.csv](assets/data.csv):

  ```sql
  CREATE TABLE gibberish
  (  ID number,
    "FIRST" text(500),
    "LAST" char(500),
    Email text,
    CategoryId int,
    ShopId int,
    PeasantId integer,
    TransactionDate date,
    VirginityLevel int,
    PricePerGig text,
    SegmentText varchar(200),
    URL varchar(200),
    BlockChainHash varchar(64)
  );
  ```

- Takhle vypadá obsah souboru

|     |           |           |                |                  |                  |                  |          |     |          |        |           |                                                                  |
| --- | --------- | --------- | -------------- | ---------------- | ---------------- | ---------------- | -------- | --- | -------- | ------ | --------- | ---------------------------------------------------------------- |
| 1   | Lucinda   | Marchal   | fesej@wagot.ws | 4050530666020864 | 5016484329816064 | 8683177851748352 | 20050430 | 4   | $6129.92 | Trikes | apbobo.gy | SCQKqfrzEojPbIoEkBOVtdhYcdXRcoAbZZnkGjsySWsZsYOlEvmmhOesHQzKTOJi |
| 2   | Catherine | Padilla   | bodbif@lo.tg   | 6860553332981760 | 1808731304099840 | 5111757070663680 | 19840317 | 3   | $9380.75 | Cars   | dam.mm    | JFIQjoTurWZjtYnesVlzmnCHDZgFhROaOZTzMDeRVyodcXFPeNNagrYhsSMxDdOd |
| 3   | Lloyd     | Comparini | puaw@tumuw.vg  | 4438045042409472 | 4171496528281600 | 6622122092789760 | 19760506 | 5   | $2939.42 | Bikes  | otace.kw  | dsHPsADRmoUcgYXkpguuGfQEOPIXxGzOjvKcVDPKWRgCJxgdzhxUQoleeSeuzHBt |

- Nebo spíš takhle

  ```csv
  1,Lucinda,Marchal,fesej@wagot.ws,4050530666020864,5016484329816064,8683177851748352,20050430,4,$6129.92,Trikes,apbobo.gy,SCQKqfrzEojPbIoEkBOVtdhYcdXRcoAbZZnkGjsySWsZsYOlEvmmhOesHQzKTOJi
  2,Catherine,Padilla,bodbif@lo.tg,6860553332981760,1808731304099840,5111757070663680,19840317,3,$9380.75,Cars,dam.mm,JFIQjoTurWZjtYnesVlzmnCHDZgFhROaOZTzMDeRVyodcXFPeNNagrYhsSMxDdOd
  3,Lloyd,Comparini,puaw@tumuw.vg,4438045042409472,4171496528281600,6622122092789760,19760506,5,$2939.42,Bikes,otace.kw,dsHPsADRmoUcgYXkpguuGfQEOPIXxGzOjvKcVDPKWRgCJxgdzhxUQoleeSeuzHBt
  ```

- Tady jsou screenshoty, které jsou buď samonavádějící, nebo s nimi pomůže lektor.

::fig[Krok 1]{src=assets/import_file_format_01.png}
::fig[Krok 2]{src=assets/import_file_format_02.png}
::fig[Krok 3]{src=assets/import_file_format_03.png}
::fig[Krok 4]{src=assets/import_file_format_04.png}
::fig[Krok 5]{src=assets/import_file_format_05.png}

...
