---
title: Zanořené podmínky
demand: 3
---

Pokračujte v kódu z předchozí úlohy. Doplňte kód do kódu doporučení pro další aktivity.

1. Přidejte proměnnou `minimalniTeplotaNaKolo` a uložte do ní hodnotu `15`.
1. Pokud nebude teplo na koupání, ale teplota dosáhne `minimalniTeplotaNaKolo`, vypište „**Čas na projížďku na kole.**“
1. Vytvořte proměnnou `bodMrazu` a uložte do ní hodnotu `0`.
1. Upravte generování předpovědi, aby byla v rozmezí `-10` až `40`.
1. Pokud bude mrznout, vypište „**Jdi postavit sněhuláka.**“
1. Pokud nebude platit žádná podmínka, vypište „**Zůstaň doma a zahraj si deskovky.**“
1. Kód zkuste několikrát pustit.

---solution

```js
let minimalniTeplotaNaKoupani = 30
let minimalniTeplotaNaKolo = 15
let bodMrazu = 0
let predpoved = -10 + Math.round(Math.random() * 50)

console.log('O víkendu bude ' + predpoved + ' °C.')

if (predpoved >= minimalniTeplotaNaKoupani) {
	console.log('Ideální příležitost jít se vykoupat.')
} else {
	if (predpoved >= minimalniTeplotaNaKolo) {
		console.log('Čas na projížďku na kole.')
	} else {
		if (predpoved <= bodMrazu) {
			console.log('Jdi postavit sněhuláka.')
		} else {
			console.log('Zůstaň doma a zahraj si deskovky.')
		}
	}
}
```
