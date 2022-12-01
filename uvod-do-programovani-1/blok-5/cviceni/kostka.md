---
title: Kostka
demand: 4
---

Napište funkci pro házení kostkou a spočítejte, jak často na ní jednotlivá čísla padají.

1. Napište funkci `hodKostkou`, která bude generovat náhodné číslo od **jedné** do **šesti**.

1. Uložte si do jednotlivých **šesti proměnných** vytvořených mimo funkci, kolikrát padla jednička, kolikrát dvojka, …

1. Pusťte funkci **desetkrát** pomocí smyčky.

1. Po provedení smyčky vypište do konzole, kolikrát které číslo padlo.

1. Upravte smyčku na **1000** cyklů.

Výsledek by měl vypadat zhruba takto:

```text
Jednička padla: 166
Dvojka padla: 150
Trojka padla: 172
Čtyřka padla: 184
Pětka padla: 157
Šestka padla: 171
```

---solution

```js
let jednicka = 0
let dvojka = 0
let trojka = 0
let ctyrka = 0
let petka = 0
let sestka = 0

let hodKostkou = () => {
	let cisloNaKostce = Math.round(0.5 + Math.random() * 6)

	if (cisloNaKostce === 1) {
		jednicka = jednicka + 1
	}
	if (cisloNaKostce === 2) {
		dvojka = dvojka + 1
	}
	if (cisloNaKostce === 3) {
		trojka = trojka + 1
	}
	if (cisloNaKostce === 4) {
		ctyrka = ctyrka + 1
	}
	if (cisloNaKostce === 5) {
		petka = petka + 1
	}
	if (cisloNaKostce === 6) {
		sestka = sestka + 1
	}
}

let pocetHodu = 0
while (pocetHodu < 1000) {
	hodKostkou()
	pocetHodu = pocetHodu + 1
}

console.log('Jednička padla: ' + jednicka)
console.log('Dvojka padla: ' + dvojka)
console.log('Trojka padla: ' + trojka)
console.log('Čtyřka padla: ' + ctyrka)
console.log('Pětka padla: ' + petka)
console.log('Šestka padla: ' + sestka)
```
