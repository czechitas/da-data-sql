---
title: Aktivity
demand: 3
---

Zabalte kód na doporučování aktivit do funkce.

1. Funkci pojmenujte `doporucAktivitu` s jedním parametrem `predpoved`.

1. Vyzkoušejte funkci zavolat pro různé hodnoty.

   - `doporucAktivitu(35)`
   - `doporucAktivitu(17)`
   - `doporucAktivitu(-5)`
   - `doporucAktivitu(9)`

---solution

```js
let doporucAktivitu = (predpoved) => {
	let minimalniTeplotaNaKoupani = 30
	let minimalniTeplotaNaKolo = 15
	let bodMrazu = 0

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
}

doporucAktivitu(35)
doporucAktivitu(17)
doporucAktivitu(-5)
doporucAktivitu(9)
```
