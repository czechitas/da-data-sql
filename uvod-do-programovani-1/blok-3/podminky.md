## Podmínky

### Základní variant

```js
let vek = 6

if (vek < 18) {
	console.log('Podmínka platí')
}
```

Reaguje pouze, pokud podmínka `(vek < 18)` platí.

### Varianta s `else`

```js
let vek = 6

if (vek < 18) {
	console.log('Podmínka platí')
} else {
	console.log('Podmínka neplatí')
}
```

Definuje, co se má stát i v případě, že podmínka neplatí.
