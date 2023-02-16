## Odkazy a důležité info

### Snowflake

- [webový klient](https://app.snowflake.com/west-europe.azure/gn56074/w2EL1gR8YIYn)
- hostname: `gn56074.west-europe.azure.snowflakecomputing.com`
- login: CZECHITA_PRIJEMNIJ (ERVINA MODRA = CZECHITA_MODRAE)
- Nemáš Czechita účet, nebo dokonce vůbec nejsi Czechita? 😱 Nevadí, stačí si založit account v [Keboola](https://connection.north-europe.azure.keboola.com/wizard) a stáhnout si [tabulku teror](https://docs.google.com/uc?export=download&id=1K60Ed4rZMDvm6CDMNHU_tIrCrMaN-3pc&confirm=true) pomocí http data source. Pak už stačí si založit snowflake workspace a dál je všechno stejné.

### Odkazy ke studiu

#### Fiddlery

Různé SQL fiddlery (místo, kde si můžete zkoušet psát dotazy nad nějakými daty a ta věc vám ukáže, co to vrací):

- [sqlfiddle](http://sqlfiddle.com/)
- [db fiddle](https://www.db-fiddle.com/)
- [a všechny další](https://www.google.com/search?q=sql+fiddle+online)

#### Challenge

Dost podobný, ale s trochou zábavy navíc, jsou všelijaký SQL challenges, kde krom toho prostředí na dotazy dostanete i nějaké úkoly k vyřešení:

- [hacker rank](https://www.hackerrank.com/domains/sql)
- [SQL ZOO](https://sqlzoo.net)
- [a všechny ostatní](https://www.google.com/search?q=sql+challenges+online)

#### Další kurzy a čtení

- [SQL pro datovou analýzu - Udacity by MODE](https://eu.udacity.com/course/sql-for-data-analysis--ud198)
- [Tutorial - Query syntax](https://docs.snowflake.net/manuals/sql-reference/constructs.html)
- [CodeAcademy](https://www.codecademy.com)
- [Super čtení](https://www.g2.com/categories/database-software) pro získání přehledu o tom, co to vlastně ty databáze jsou, kdo je vyrábí, proč jich je tolik druhů, kdo je prodává a tak vůbec
- [o joinech](https://dataschool.com/how-to-teach-people-sql/inner-join-animated)
- [o joinech podruhé](https://towardsdatascience.com/can-we-stop-with-the-sql-joins-venn-diagrams-insanity-16791d9250c3)

### TEROR

- [Komplet dokumentace](assets/gtd.pdf)
- Dataset zobrazuje teroristické útoky v letech 1970 - 2021, kde 1 řádek reprezentuje 1 incident.
- Dataset obsahuje přes 200 tisíc řádků a 135 sloupců.

|          **Sloupec**           |                                                    **Popis**                                                     |                                                           **Detail**                                                           |
| :----------------------------: | :--------------------------------------------------------------------------------------------------------------: | :----------------------------------------------------------------------------------------------------------------------------: |
|            eventid             |                                        12-místné ID incidentu (události)                                         |                    prvních 8 čísel = datum záznamu, poslední 4 čísla = pořadové číslo případu pro daný den                     |
|             iyear              |                                         rok, ve kterém k incidentu došlo                                         |                                                                                                                                |
|             imonth             |                                        měsíc, ve kterém k incidentu došlo                                        |                                                                                                                                |
|              iday              |                                         den, ve kterém k incidentu došlo                                         |                                                                                                                                |
|           approxdate           | v případě, že není známo přesné datum incidentu nebo je nejasné, toto pole slouží k zaznamenání přibližného data |                                                                                                                                |
|            extended            |                                         incident trval déle než 24 hodin                                         |                                                      1 = "Ano", 0 = "Ne"                                                       |
|           resolution           |                                zaznamenává datum, ve kterém byl incident vyřešen                                 |                                             pokud pole 'Extended' je rovno 1 (Ano)                                             |
|      country; country_txt      |                                      země nebo místo, kde k incidentu došlo                                      |                            v případě, že zemi nelze identifikovat, je místo označené jako 'Unknown'                            |
|       region; region_txt       |                                          region, kde k incidentu došlo                                           |                                                          12 kategorií                                                          |
|           provstate            |                                            provincie, správní region                                             |                                                                                                                                |
|              city              |                                           město, kde k incidentu došlo                                           |                                                                                                                                |
|            latitude            |                                zeměpisná šířka města, ve kterém došlo k incidentu                                |                                                                                                                                |
|           longitude            |                                zeměpisná délka města, ve kterém došlo k incidentu                                |                                                                                                                                |
|   attacktype; attacktype_txt   |                                                    typ útoku                                                     |                              9 kategorií; pro každý incident lze zaznamenat až 3 typy útoků (1-3)                              |
|            success             |                                        zaznamenává, zda byl útok úspěšný                                         |                    1 = "Ano", 0 = "Ne"; definice úspěšného útoku závisí na typu útoku, zda byl zrealizován                     |
|            suicide             |            zaznamenává případ, kdy existuje důkaz, že pachatel neměl v úmyslu uprchnout z útoku živý             |                                                      1 = "Ano", 0 = "Ne"                                                       |
|     weaptype; weaptype_txt     |                                                    typ zbraně                                                    |                                      13 kategorií; lze zaznamenat až 4 typy zbraní (1-4)                                       |
|  weapsubtype; weapsubtype_txt  |                                                  podtyp zbraně                                                   |                                                                                                                                |
|    targtype; targtype1_txt     |                                                 typ oběti / cíle                                                 |                                       22 kategorií; lze zaznamenat až 3 typy obětí (1-3)                                       |
| targsubtype1; targsubtype1_txt |                                           konkrétnější cílová skupina                                            |                                                                                                                                |
|              corp              |                                                  název subjektu                                                  |                                       PO nebo vládní agentura, na kterou byl útok cílen                                        |
|             target             |                                              specifická oběť / cíl                                               | konkrétní osoba, budova, instalace apod., na kterou byl útok zaměřen a/nebo byla obětí a je součástí výše jmenovaného subjektu |
|      natlty1; natlty1_txt      |                                         státní příslušnost oběti / cíle                                          |                                                                                                                                |
|             gname              |                                 název skupiny / organizace , která útok provedla                                 |                                           lze zaznamenat až 3 různé pachatele (1-3)                                            |
|            gsubname            |                                        další podrobnosti o názvu skupiny                                         |                                                                                                                                |
|           individual           |                                             nepřiřazení jednotlivci                                              |                                                      1 = "Ano", 0 = "Ne"                                                       |
|            claimed             |                                       přihlášení se k odpovědnosti za útok                                       |                        1 = "Ano", 0 = "Ne"; váže se k organizaci, ke každé (1-3) lze hodnotu zaznamenat                        |
|    claimmode; claimmode_txt    |                                                způsob přihlášení                                                 |                                                           10 režimů                                                            |
|             motive             |                                                      motiv                                                       |                                                                                                                                |
|             nkill              |                                              celkový počet mrtvých                                               |                          zahrnuje všechny oběti a útočníky, kteří zemřeli v přímém důsledku incidentu                          |
|            nkillus             |                                           počet usmrcených občanů USA                                            |                                                                                                                                |
|            nkillter            |                                             počet mrtvých teroristů                                              |                                                                                                                                |
|             nwound             |                                             celkový počet zraněných                                              |                                                                                                                                |
|            nwoundus            |                                            počet zraněných občanů USA                                            |                                                                                                                                |
|            nwoundte            |                                            počet zraněných teroristů                                             |                                                                                                                                |
|            property            |                           existují-li důkazy o škodách na majetku v důsledku incidentu                           |                                              1 = "Ano", 0 = "Ne", -9 = "Neznámo"                                               |
|           ishostkid            |                              zaznamenává, zda byli zajati růkojmí či došlo k únosu                               |                                              1 = "Ano", 0 = "Ne", -9 = "Neznámo"                                               |
|            nhostkid            |                                      celkový počet rukojmích či obětí únosu                                      |                                                                                                                                |
|           nhostkidus           |                           počet občanů USA, kteří byly uneseni či zajati jako rukojmí                            |                                                                                                                                |
|             nhours             |                 zaznamenává dobu trvání incidentu v případě, že událost trvala méně než 24 hodin                 |       pokud je typ útoku “Hostage Taking (Kidnapping),” “Hostage Taking (Barricade Incident),” nebo úspěšný “Hijacking”        |
|             ndays              |                 zaznamenává dobu trvání incidentu v případě, že událost trvala déle než 24 hodin                 |       pokud je typ útoku “Hostage Taking (Kidnapping),” “Hostage Taking (Barricade Incident),” nebo úspěšný “Hijacking”        |

### Slack/gpt3

Líná huba ... ale co to, prostě žádná otázka není hloupá. Dá se zeptat lektorů a koučů [na slacku](https://czechitasdadata.slack.com/archives/C033B2YEUBY/p1645094665704339). Anebo [vševědoucího robota](https://chat.openai.com/chat). Nedělám si legraci, v minulém běhu jsem vyzkoušel robotovi položit všechny otázky, které kolem SQL padly na slacku a na všechny odpověděl. Pár příkladů:
::fig[Robot pomáhá]{src=assets/blbnicek-chatgpt-01.png}

::fig[Robot pomáhá]{src=assets/blbnicek-chatgpt-02.png}

::fig[Robot pomáhá]{src=assets/blbnicek-chatgpt-03.png}

### Tahák

Kdysi dávno Monika udělala [tahák na SQL](https://docs.google.com/document/d/1s3wOvk8n5ZOkp_wc5oep7U961sh0A6GxgXH_v0QFqcE) a ten se i teď může ledaskomu hodit.

### Chybí tu něco?
