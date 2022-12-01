---
title: Větší než nebo záporné
demand: 2
---

1. Vytvořte proměnnou, do které načtete číslo od uživatele z klávesnice.
1. Zadané číslo vypište do konzole ve formátu „**Uživatel zadal číslo -15**“.
1. Pokud je hodnota této proměnné větší než `100`, vypište „**Číslo je větší než 100**“.
1. A pokud je hodnota této proměnné menší než nula, vypište „**Číslo je záporné**“.
1. Program vyzkoušejte na různých hodnotách.

---solution

```js
let cislo = Number(prompt('Zadej číslo:'))

console.log('Uživatel zadal číslo ' + cislo)

if (cislo > 100) {
	console.log('Číslo je větší než 100')
}

if (cislo < 0) {
	console.log('Číslo je záporné')
}
```
