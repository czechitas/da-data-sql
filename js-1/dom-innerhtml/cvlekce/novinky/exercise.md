---
title: Novinky
demand: 2
---

Naklonujte si [repozitář](https://github.com/Czechitas-podklady-WEB/novinky-zadani) s připravenou webovou stránku, která zobrazuje několik zajímavých zpráv. Zobrazte stránku v prohlížeči a veškeré úkoly z tohoto cvičení proveďte v JavaScriptové konzoli.

1. Pomocí `document.querySelector` vyberte element `body` a uložte si jej do proměnné `bodyElm`. Pomocí této proměnné nastavte barvu pozadí elementu na hodnotu `#e9e9e9`.
1. Do jiné proměnné vyberte element s třídou `news` a nastavte mu barvu pozadí na bílou a maximální šířku na `60rem`.
1. Vyberte element `h1` a nastavte mu třídu na `news__title`. Nadpis by měl změnit styl. Nastavte také obsah nadpisu na text `Aktuální novinky`.
1. Pomocí atributu `id` vyberte element obsahující první zprávu. Přidejte do jeho atributu `class` třídu `post--main`. První zpráva by tak měla mírně změnit svůj styl.
1. Vyberte obrázek v poslední zprávě a změnte jeho atribut `src` na obrázek `img/zprava3-novy.jpg`.

Na konci by stránka v prohlížeči měla vypadat jako na obrázku níže:

::fig[Snímek obazovky s řešením]{src=assets/screen-novinky.png}

---solution

```js
// a.
const bodyElm = document.querySelector('body')
bodyElm.style.backgroundColor = '#e9e9e9'

// b.
const newsElm = document.querySelector('.news')
newsElm.style.backgroundColor = 'white'
newsElm.style.maxWidth = '60rem'

// c.
const headingElm = document.querySelector('h1')
headingElm.classList.add('news__title')
headingElm.textContent = 'Aktuální novinky'

// d.
const firstPostElm = document.getElementById('zprava1')
firstPostElm.classList.add('post--main')

// e.
const lastPostImgElm = document.querySelector('#zprava3 img')
lastPostImgElm.src = 'img/zprava3-novy.jpg'
```
