---
title: Desetinné číslo
demand: 2
---

Vytvořte proměnnou a přiřaďte do ní **číslo s desetinným rozvojem** (např. `9.823`). Dále vytvořte druhou proměnnou a **zaokrouhlete** do ní původní desetinné číslo. Obě hodnoty **vypište do konzole**.

---solution

```js
let pi = 3.14159265358979
let poZaokrouhleni = Math.round(pi)
console.log('Před zaokrouhlením: ' + pi)
console.log('Po: ' + poZaokrouhleni)
```
