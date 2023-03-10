---
title: 1.4 Unikátní roky
demand: 1
---

Vypiš všechny roky vyskytující se v tabulce teror, tak aby byl každý rok ve výsledné tabulce jen jednou.

---solution

Abychom vybrali pouze jeden sloupec, napíšeme jeho jméno:

```sql
  SELECT DISTINCT IYEAR
  FROM TEROR;
```

Anebo je všechny _zgroupovat_ podle IYEAR:

```sql
  SELECT IYEAR
  FROM TEROR
  GROUP BY IYEAR;
```

Tato varianta bude fungovat, ale je zbytečně složitá a pokud se nedotazujete na nic složitějšího, tak nedoporučujeme. 😉
