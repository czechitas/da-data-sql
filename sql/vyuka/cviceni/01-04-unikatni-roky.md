---
title: 4. Unikátní roky
demand: 1
---

Vypiš všechny roky vyskytující se v tabulce teror, tak aby byl každý rok ve výsledné tabulce jen jednou

---solution

Abychom vybrali všechny sloupce, můžeme použít buď hvězdičku:

    ```sql
    SELECT DISTINCT IYEAR FROM TEROR;
    ```

Anebo je všechny _zgroupovat_ podle IYEAR:

    ```sql
    SELECT IYEAR

    FROM TEROR

    GROUP BY IYEAR;
    ```
