---
title: Větší než
demand: 2
---

1. Vytvořte proměnnou, do které vložíte výsledek z generátoru náhodných čísel.
1. Vypište hodnotu této proměnné.
1. Pokud její hodnota bude větší než `0.5`, vypište text „**Číslo je větší než 0.5**“.
1. Program zkuste několikrát pustit a koukejte, jestli se přibližně v polovině případů text vypíše.

---solution

```js
let nahodneCislo = Math.random()

console.log('Náhodné číslo je ' + nahodneCislo)

if (nahodneCislo > 0.5) {
	console.log('Číslo je větší než 0.5')
}
```
