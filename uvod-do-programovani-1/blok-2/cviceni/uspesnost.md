---
title: Úspešnost
demand: 2
---

Vytvořte proměnnou `uspesnost`, do které vložíte číslo z generátoru náhodných čísel a upravte ho tak, aby hodnota byla **celočíselná** v **rozmezí od `0` do `100`**. Hodnotu této proměnné vypište **do konzole** ve formátu „**Úspěšnost: 84 %**“.

---solution

```js
let uspesnost = Math.round(Math.random() * 100)

console.log('Úspěšnost: ' + uspesnost + ' %')
```
