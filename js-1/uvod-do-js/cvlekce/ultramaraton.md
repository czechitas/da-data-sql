---
title: Ultramaraton
demand: 2
---

Představme si, že jste pořadatelé ultramaratonského závodu. Závod začíná ve **tři hodiny odpoledne**, což ve 24-hodinovém formátu zapíšeme jako **15**. Nejlepší běžec zvládne vaši brutální trasu za **10 hodin**. Doběhne tedy **v jednu hodinu ráno**, v našem formátu zapsáno jako **1**.

Založte si JavaScriptový program a uložte čas startu závodu do proměnné `start`. Do proměnné `delka` uložte délku závodu pro nějakého běžce. Klidně může být pomalejší než náš šampion. Do proměnné `konec` spočítejte, v kolik hodin závod pro našeho běžce skončí a vypište její obsah do stránky. Vyzkoušejte různé délky a ověřte, že váš postup funguje.

---solution

```js
const start = 15
const delka = 10
const konec = (start + delka) % 24
document.body.innerHTML = 'Čas konce v hodinách: ' + konec
```
