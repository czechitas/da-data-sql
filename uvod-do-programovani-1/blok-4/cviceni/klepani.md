---
title: Klepání
demand: 2
---

Pomocí dvou vnořených cyklů vypište všechna Sheldnova klepání.

Výsledek by měl vypadat takto:

```text
Knock
Knock
Knock
Penny!
Knock
Knock
Knock
Penny!
Knock
Knock
Knock
Penny!
```

---solution

```js
let pocetPenny = 0
while (pocetPenny < 3) {
	let pocetUderu = 0

	while (pocetUderu < 3) {
		pocetUderu = pocetUderu + 1
		console.log('Knock')
	}
	console.log('Penny!')

	pocetPenny = pocetPenny + 1
}
```
