---
title: Množstevní slevy
demand: 2
---

Firma nabízející trička s potiskem poskytuje množstevní slevy podle počtu objednaný kusů. Při objednávce **pod 50 kusů** stojí **jedno tričko 300 Kč**. Pokud si objednáme **alespoň 50 kusů**, je cena **250 Kč** za kus. Při objednávce **nad 200 kusů** je cena **200 Kč** za tričko. **Nad 500 kusů** zaplatíme **150 Kč** za tričko a **nad 1000 kusů** zaplatíme **120 korun** za tričko.

Napište stránku, která od uživatele obdrží **počet kusů**, které si chce objednat, a program **odpoví celkovou ceny objednávky** výpisem do stránky.

---solution

```js
const pocetKusu = Number(prompt('Zadejte počet kusů.'))

let cena

if (pocetKusu < 50) {
	cena = pocetKusu * 300
} else if (pocpocetKusu < 200) {
	cena = pocetKusu * 250
} else if (pocetKusu < 500) {
	cena = pocetKusu * 200
} else if (pocetKusu < 1000) {
	cena = pocetKusu * 150
} else {
	cena = pocetKusu * 120
}

document.body.innerHTML = `<p>Výsledná cena je ${cena} Kč.</p>`
```
