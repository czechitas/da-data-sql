---
title: Koupání
demand: 3
---

Napište kód, který bude podle počasí doporučovat aktivity na víkend.

1. Vytvořte proměnnou `minimalniTeplotaNaKoupani` a uložte do ní hodnotu `30`. Hodnotu případně upravte podle vašich preferencí.
1. V dalším kroku zjistěte předpověď na víkend. Vytvořte proměnnou `predpoved` a uložte do ní náhodně generované celé číslo mezi `0` a `40`.
1. Vypište do vývojářské konzole, kolik bude o víkendu stupňů ve formátu „**O víkendu bude 17 °C.**“
1. Pokud bude předpověď větší nebo rovna `minimalniTeplotaNaKoupani`, vypište „**Ideální příležitost jít se vykoupat.**“
1. Kód zkuste několikrát pustit.

---solution

```js
let minimalniTeplotaNaKoupani = 30
let predpoved = Math.round(Math.random() * 40)

console.log('O víkendu bude ' + predpoved + ' °C.')

if (predpoved >= minimalniTeplotaNaKoupani) {
	console.log('Ideální příležitost jít se vykoupat.')
}
```
