## Bonus: Zanořené podmínky

### Počasí

```js
let teplota = 27
let srazky = 'ano'

if (srazky === 'ano') {
	if (teplota > 0) {
		console.log('Venku prší.')
	} else {
		console.log('Venku sněží.')
	}
} else {
	console.log('Venku neprší ani nesněží.')
}
```

### Věk

```js
let vek = 6

if (vek >= 18) {
	console.log('Je dospělá')
} else {
	if (vek > 10) {
		console.log('Je náctiletá')
	} else {
		console.log('Je dítě')
	}
}
```
