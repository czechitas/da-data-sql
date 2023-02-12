## Databázové servery, příkazy, dotazy objekty atd.

### Databázový server

O každého by se měl někdo postarat, když má rýmu, nepouštět k němu neohlášené návštěvy, uvařit čaj a tak. O databáze se stará server. I když rýmu zrovna nemají. Mezi tebou a databázemi stojí server jako gardedáma a tlumočník v jedné osobě. Může se starat o jedinou databázi, nebo taky o gazilión databází. Když ho požádáš o data z tabulky v databázi, zjistí nejdřív, jak se ta databáze cítí, jestli je ready. Když jsou v databázi požadovaná data a máš k nim ta správná oprávnění, server do ní pošle tvůj dotaz a vrátí ti výsledek. Rozsah detailů, podle kterých se dají v databázích oprávnění nastavovat, je neuvěřitelný. Konkrétní oprávnění lze nastavovat na všechny objekty, čtení/zápis, ale i na sloupečky nebo řádky v tabulkách. Serverů existuje mnoho druhů, klanů, mluví různými nářečími (flavors), hotová mafie, ale o tom později.

### Příkazy

Příkaz je SQL kód, kterým server nutíš k nějaké činnosti. Jsou jich dva druhy a ty se ještě dál dělí. Mají krásná jména - **DML** a **DDL**. Je to snadné, **Data Definition Language** vytváří, mění a maže tabulky a ostatní objekty v databázi, zatímco **Data Manipulation Language** vybírá, upravuje a maže data z/v tabulkách.

Příkazy DDL:

- `CREATE`
- `ALTER`
- `DROP`

Příkazy DML:

- `SELECT`
- `INSERT`
- `UPDATE`
- `DELETE`

A teď hádanka, který příkaz co dělá (tohle nebude v testu)? Tady je SQL ještě opravdu srozumitelné na první pohled.

### Dotazy

`SELECT` je dotaz, dotaz je query. Tohle bude tvůj nejvíc nejlepší kamarád v praktické části, “selektovat” budeš až se ti bude kouřit od klávesnice. Důvod? Analytici se daty většinou přehrabují tam a zpátky jako design-influencerky v antiq shopech. Vytváření tabulek a jejich plnění daty mají na svědomí většinou aplikace (vývojáři) anebo tajemný data warehouse (BI vývojáři). Ale neboj, datoví analytici, vědkyně a všechna ta datová kavárna si z nich stejně vytváří svoje tabulky, dočasné tabulky a pomocné tabulky. Tvorbu tabulek a plnění daty si zažiješ v hodinách jen okrajově, v projektech už o poznání víc.

### Data lifecycle

Tohle bude trochu delší. Data jako ostatně všechno ve vesmíru nějak vznikají, existují a nakonec i zanikají. V průběhu jejich existence nebo vznešeněji života je používají různí lidé a stroje k různým účelům.

Začíná to aplikací, jedno, jestli je to pokladna v Bille, Tinder, nebo realtime monitoring Tesla, každá appka pod sebou má databázi, kde je uloženo minimálně to, co mají uživatelé vidět a co do ní vkládají. Jasně, uživatel může být auto i prodavačka. A protože žijeme ve zvědavé době, ukládá se do databází i to, jak uživatelé aplikaci používají, jak v aplikaci dochazí k chybám a vlastně úplně všechno.

Paráda, všechno v appce máme. Každý vidí anebo ukládá, co má. Jenže. Jenže vždycky se najde někdo, kdo by chtěl vědět, kolik máme zákazníků, nákupů, vozidel, výpadků atd. Samozřejmě, že v aplikacích jsou různé přehledy (dashboardy), reporty a tak. Dřív nebo později ale někdo chce data vidět jinak. Spojit je s daty z jiné aplikace, kupříkladu účetnictví. A tak se data přelijí do místa, kde se dají mučit donekonečna a všemi metodami. Do data martu, nebo datového skladu, neboli data warehouse (DWH). Důvodů je několik. Když se analytik zeptá přímo do aplikační databáze Billy, kterých 10 prodejen mělo v kterých měsících největší meziměsíční nárůst prodejů mléčných výrobků se zahrnutím vlivu růstu jejich ceny, zní to jako legitimní dotaz. Ale aplikační databáze je stavěná na veliké množství "malých" dotazů, respektive dotazů, které rychle vrátí, nebo upravují malé množství řádek. Takovýhle analytický dotaz naproti tomu několikrát prošmejdí celou databázi křížem krážem aby nakonec ukázal deset prodejen. Může to trvat minuty, hodiny, nebo i déle. A po celou tu dobu můžou ve skladech skladníci koukat na točící se kolečka na čtečkách, pokladní na "waiting..." na kasách a tak. Když ten samý dotaz pustí analytik ve warehousu, neovlivní nijak provoz aplikace. Další výhodou warehousu je, že se v něm nachází i data z ostatních systémů, nebo společností v Aholdu tak maji na jedné hromadě Alberty, Hyperalberty, Spar, Interspar, Manu a vlastně všechno, co kdy aspoň na chvilku vlastnili.

Když máme data ve skladu, najednou po nás každý chce analýzy. BI kavárna je stejně líná, jako ta programátorská a tak většinou při třetí stejné analýze vznikne nějaký report, nebo dashboard a v něm ať si kdo chce drilldownuje do haleluja. Po pár letech je v tom takový guláš (protože různé reporty ukazují totéž tolika rozmanitými způsoby), že už i otrlému business pankáčovi to přijde moc a vznikne nějaký datový katalog, bible, korán, dokumentace. Tam je vysvětleno jednotným jazykem (korporátní newspeak, geekovština, úředničtina...), co která hodnota (tabulka, sloupecek, KPI) znamená, jak vzniká a který report slouží pro koho a k čemu.

Umřou někdy data? Všichni umřeme. Data o prodejích z devadesátek už asi nikdo k ničemu nepoužije a tak se prostě zahodí. Data o návštěvnících B2B portálu, na co klikali, kde strávili kolik času atp., ty asi můžeme zahodit ještě dřív. Zahazování se učeně říká retence. U některých dat je skoro nekonečná, např. u lékařských záznamů, ale najdou se i jiné příklady.

### Objekty

O tabulkách a databázích už víš. Většina databází si organizuje tabulky do schémat, aby se v nich dalo lépe orientovat. Ale tabulky nejsou to jediné, co může být v databázi uložené. Analytici, programátoři, možná všichni lidi jsou líní. A tak, když už horko těžko vypotí nějaký dotaz, nechtějí ho psát podruhé. Uloží si ho do databáze pod nějakým jménem a voila, view (aka uložený pohled) je na světě. A podobně jako views můžou být v databázi uložené indexy, funkce, procedury, sekvence, synonyma, materializované pohledy a spousta dalšího pekla. Spíš? Jsi tu ještě? Haló! Teď asi nastala ta chvíle, kdy se mnou budeš souhlasit, že přeskočení teorie a hození do divoké řeky SELECTů, nebyl úplně nejhorší nápad. Klid, už jsme za půlkou ;)

- ::fig[Objekty v databázi]{src=assets/teorie-objects.png}

### Engine

Engine aka databázový server je vlastně jenom šikovná aplikace, která databáze zpřístupňuje. A existuje hodně variant těchto serverů. Některé jsou zadarmo (PostgreSQL, SQLite, MariaDB...), jiné za peníze (Oracle, MS SQL, MySQL, Enterprise DB...). Aby to nebylo tak snadné, nabízí Oracle free verzi Oracle DB XE, dále je k dispozici Microsoft SQL Express, MariaDB je zase zadarmózní (brr..) klon MySQL a EnterpriseDB je placená verze PostgreSQL.

Kromě rozdělení na placené a free jde ještě databázové servery dělit podle mnoha dalších kritérií. Každá varianta se může hodit na něco lépe a na něco, jak říkají američani, už ne tak dobře. Všechny zmíněné databázové servery je potřeba někam instalovat, respektive mít pro ně server, kde poběží. Snowflake je reprezentantem tzv. cloudových databází, které se sice "nikam neinstalují", ale jen někomu dáš číslo své kreditky a platíš za místo, výkon a podobně, takže to někdo celé zařídí za tebe. Microsoft a Oracle také nabízí svoje enginy v cloudu, Heroku je zase postgres v cloudu, AWS má svůj Redshift, Aurora, DynamoDB atd.

### Flavors

SQL je standardizovaný strukturovaný dotazovací jazyk. Standardizaci si samozřejmě každý výrobce/vývojářská komunita vysvětluje jinak. A tak existují flavors. Flavor je něco jako nářečí SQL jazyka, někdy se mu taky říká SQL dialect. Každý k SQL standardu přilepuje svoje super featury. V SQL jako v každém jazyce existují různé funkce, například takové, které pracují s datumem. Funkce ale nejsou součástí standardu, takže získat z datumu rok, měsíc a den pravděpodobně dopadne trochu jiným dotazem v každém enginu. Doporučuju se s tím smířit. Přechod mezi dvěma flavors je pro člověka jednoduchý - je to jako přechod z americké angličtiny na australskou, těžší to mají aplikace, které využívají nějaký konkrétní engine. U nich to je přechod spíš jako dostat do Oktávky motor z Chevroletu. Ještě jednou pro uklidnění, když se naučíš SQL ve Snowflake, přejít na MSSQL, Oracle, Postgres nebo MySQL bude brnkačka. 💙

- ::fig[Engines]{src=assets/teorie-engines.png}

### Rezervovaná slova a další odlišnosti

Ještě se vrátím k rozdílům mezi dialekty/flavors. Aby ti engine rozuměl, má některá slova tzv. rezervovaná, tj. nejde je použít pro názvy sloupečků, tabulek apod. Příkladem takových rezervovaných slov je třeba table, column, view, date, int apod. V rezervovaných slovech se různé flavory (implementace) taky liší. Ale klid, nic zásadního. Některé enginy rozlišují v názvech objektů velká a malá písmena, jiné ne. Některé jsou velmi omezené v délce těchto názvů (nejmenší, co pamatuju, je 30 znaků limit na název v DB2 od IBM, Postgres má limit 63).

::fig[Zakázaná slova]{src=assets/teorie-zakazana-slova.png}
