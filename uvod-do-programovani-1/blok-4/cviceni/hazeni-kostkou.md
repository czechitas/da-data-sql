---
title: Házení kostkou
demand: 2
---

Vytvořte program, který bude virtuálně házet kostkou, dokud mu nepadne 6. Každý hod vypíšte na obrazovku.

Výsledek by měl vypadat zhruba takto:

```text
Padlo číslo 3
Padlo číslo 2
Padlo číslo 5
Padlo číslo 3
Padlo číslo 1
Padlo číslo 6
```

---solution

```js
let cisloNaKostce = 0
while (cisloNaKostce !== 6) {
	cisloNaKostce = Math.round(0.5 + Math.random() * 6)
	console.log('Padlo číslo ' + cisloNaKostce)
}
```
