## Cvičení k lekci 5

Importujte [ukol.csv](assets/ukol.csv) do Snowflake. Vytvořte si tabulku, která bude obsahovat sloupce jako v csv souboru. Pojmenujte si ji dle libosti. Prohlédněte si nejdříve data třeba v Excelu (Podívejte se na datové typy. NÁPOVĚDA: Datum není datum, importujte jako string).
Importujte data do tabulky. Pozor budete si muset vytvořit vlastní file format!!!

#### Soubor [ukol.csv](assets/ukol.csv)

```csv
id,full name,email,gender,birth Date,address,city,Country,Postal Code
1,Gerry Antonutti,gantonutti0@amazonaws.com,Male,4. 9. 1987,2496 Gerald Point,Houxixi,China,332140
2,Allin Haward,ahaward1@pcworld.com,Male,13. 12. 2001,0 Dakota Road,Xuanhuadian,China,
```

| id  | full name       | email                     | gender | birth Date   | address                | city        | Country | Postal Code |
| --- | --------------- | ------------------------- | ------ | ------------ | ---------------------- | ----------- | ------- | ----------- |
| 1   | Gerry Antonutti | gantonutti0@amazonaws.com | Male   | 4. 9. 1987   | 2496 Gerald Point      | Houxixi     | China   | 332140      |
| 2   | Allin Haward    | ahaward1@pcworld.com      | Male   | 13. 12. 2001 | 0 Dakota Road          | Xuanhuadian | China   |             |
| 3   | Burtie Warbrick | bwarbrick2@lulu.com       | Male   | 16. 2. 1986  | 98 Westerfield Parkway | Pieńsk      | Poland  | 59-930      |

Pokud nevíte, jak naimportovat data, nebo jste se na tom zasekli, tak [tady je podrobný návod](https://docs.google.com/document/d/11dnUti4Uw2-wMRC3Ij4SJLHynxZGDPS557paIpYnUvk/edit?usp=sharing). **Doporučujeme** si to nejprve zkusit sami, a pokud jste v koncích 😓, tak nakouknout do návodu a projít ho do bodu, kde jste narazili na chybu ❌. Třeba si najdete chybku po cestě. 🙌 

**Nedoporučujeme** se na to vykašlat a jet rovnou podle návodu, z toho si odnesete velmi málo. 😉

::exc[cviceni/05-01]
::exc[cviceni/05-02]
::exc[cviceni/05-03]
::exc[cviceni/05-04]
::exc[cviceni/05-05]
