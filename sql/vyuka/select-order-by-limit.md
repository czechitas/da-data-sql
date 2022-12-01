## SELECT, ORDER BY, LIMIT

### První select

- klíčová slova
- velikost PíSMen

```sql
SELECT 1;   -- vybere 1
```

### Kalkulačka

- sčítání
- odčítání
- násobení
- všechno funguje, jak má

```sql
SELECT 25 * 30; -- 750
SELECT 25 * 0;  -- 0
SELECT 25 / 2.5; -- 10.0
SELECT 25/0; -- division by zero
```

### Všechny sloupečky a řádky

Prostě vybereme všechno.

```sql
SELECT * FROM sch_czechita.teror; -- vybere vsechny sloupce a radky z tabulky teror
```
