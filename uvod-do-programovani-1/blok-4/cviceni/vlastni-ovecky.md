---
title: Vlastní ovečky
demand: 3
---

Pokračujte v předchozí úloze s počítáním oveček. Pomocí funkce `prompt` se uživatele zeptejte, kolik oveček chce spočítat a upravte podle toho i podmínku cyklu.

---solution

```js
let ovecka = 1
let cilovyPocet = Number(prompt('Kolik oveček chceš spočítat?'))

while (ovecka <= cilovyPocet) {
	console.log('Proběhla ' + ovecka + '. ovečka')
	ovecka = ovecka + 1
}
```
