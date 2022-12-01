---
title: Počítání oveček
demand: 3
---

Vytvořte program na uspávání, který pomocí cyklu vypíše první čtyři ovečky následovně:

```text
Proběhla 1. ovečka
Proběhla 2. ovečka
Proběhla 3. ovečka
Proběhla 4. ovečka
```

---solution

```js
let ovecka = 1
while (ovecka <= 4) {
	console.log('Proběhla ' + ovecka + '. ovečka')
	ovecka = ovecka + 1
}
```
