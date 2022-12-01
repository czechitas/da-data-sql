---
title: Zajímavost
demand: 2
---

Vytvořte funkci, která bude vypisovat zajímavosti.

1. Vytvořte funkci `rekniZajimavost`, která po spuštění vypíše do konzole jednu krátkou zajímavost. Třeba „**JavaScript je nejlepší programovací jazyk.**“ (Bude vždy stejná.)
1. Rozšiřte vaší funkci repertoár. Nechte funkci náhodně vypsat jednu ze dvou zajímavostí za použití podmínky `Math.random() < 0.5`.
1. Přidejte třetí zajímavost a rozšiřte podmínku tak, aby na třetí zajímavost také mohla někdy vyjít řada.
1. Funkci si zkuste párkrát zavolat.

---solution

```js
let rekniZajimavost = () => {
	if (Math.random() < 0.33) {
		console.log('JavaScript je nejlepší programovací jazyk.')
	} else {
		if (Math.random() < 0.5) {
			console.log('Webové stránky se píší v jazyce HTML, CSS a JS.')
		} else {
			console.log('Díky JavaScriptu mohou být stránky interaktivnější.')
		}
	}
}

rekniZajimavost()
rekniZajimavost()
rekniZajimavost()
rekniZajimavost()
rekniZajimavost()
```
