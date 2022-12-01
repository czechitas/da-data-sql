---
title: Bonus
demand: 2
---

#### Kostka

Vytvořte program, který vypíše text podle vzoru „**Hozeno na kostce: 5**“, s tím, že hozené číslo se bude generovat náhodně. Musíte vyřešit, jak z náhodného čísla v rozsahu `0` až `0.9999999` dostanete číslo `1` až `6`.

#### Sčítačka

Vytvořte program na sčítání.

1.  Požádejte uživatele o první číslo výpisem jako je např. „Zadej první číslo:“, hodnotu uložte do proměnné.
1.  Podobně načtěte **druhé číslo**.
1.  Vypište uživateli výsledek ve formátu „**5 + 8 = 13**“.

---solution

#### Kostka

```js
let cisloNaKostce = Math.round(0.5 + Math.random() * 6)
console.log('Hozeno na kostce: ' + cisloNaKostce)
```

#### Sčítačka

```js
let prvni = Number(prompt('Zadej první číslo:'))
let druhe = Number(prompt('Zadej druhé číslo:'))

console.log(prvni + ' + ' + druhe + ' = ' + (prvni + druhe))
```
