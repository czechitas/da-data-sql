## JavaScriptové recitály

Jedním z hlavních způsobů, jak si ušetřit mnohé frustrace a mlácení hlavou o stůl během programování, je naučit se doopravdy porozumět tomu, co děláte. V předchozích lekcích se na vás vyhrnulo velké množství nových pojmů a možná se mezi nimi zmítáte jako vratká bárka v rozbouřeném moři. Zkusíme tedy postupně zakotvit tím, že se budeme učit přesně popsat - takzvaně recitovat - co přesně dělá nějaký kousek kódu, aby si vás mozek zvyknul na JavaScriptové myšlení.

### Recitál první

Mějme například následující kousek kódu.

```js
const input = prompt('Username:')
```

Tento úryvek můžeme po technické stránkce rozebrat takto.

::fig[Recitál první]{src=assets/recitation01.svg}

Dle tohoto rozboru pak můžene sestavit následující technický popis.

> Vytváříme proměnnou `input`, do které ukládáme výsledek volání funkce `prompt`. Této funkci jako vstup (argument) předáváme řetězec `'Username:'`.

Vedle technického popisu také zkusíme odhadnout záměr uvedeného kódu.

> Získáváme vstup od uživatele a chceme po něm jeho uživatelské jméno.

### Recitál druhý

Zkusme nyní malinko složitější úryvek.

```js
const setColor = (element, color) => {
	element.style.color = color
}
```

Technický rozbor by mohl vypadat takto.

::fig[Recitál druhý]{src=assets/recitation02.svg}

> Vytváříme proměnnou `setColor`, do které ukládáme novou funkci se dvěma parametry. Tato funkce vezme hodnotu v parametru `element` a nastaví vlastnost `color` vlastnosti `style` na hodnotu uloženou v parametru `color`.

Záměr funkce můžeme odhadnout takto.

> Funkce `setColor` nastavuje barvu textu zadaného elementu na zadanou barvu.
