---
title: Objednávka
demand: 1
---

Vytvoříme jednoduchou stránku s objednávacím tlačítkem.

1. Založte HTML stránku s jedním tlačítkem
   ```html
   <button id="btn-order">Objednat</button>
   ```
1. Doplňte do stránky JavaScriptový program, který zařídí, že po stisknutí tlačítka se do stránky za tlačítko vypíše odstavec:
   ```html
   <p>Objednáno</p>
   ```
1. Upravte program tak, že text se nevypíše do stránky, ale zobrazí se na samotném tlačítku.

::fig[ukázka řešení]{src=assets/ukazka.gif}

---solution

```js
const button = document.querySelector('#btn-order')
button.addEventListener('click', () => {
	// document.body.innerHTML += '<p>Objednáno</p>';
	button.textContent = 'Objednáno'
})
```
