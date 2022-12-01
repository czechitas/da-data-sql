## Funkce

### Základní

#### Kód

```js
let pozdrav = () => {
	console.log('Ahoj světe')
}

pozdrav()
```

#### Výsledek

```text
Ahoj světe
```

### S parametrem

#### Kód

```js
let pozdrav = (jmeno) => {
	console.log('Ahoj ' + jmeno)
}

pozdrav('Honza')

let neciJmeno = 'Jarda'
pozdrav(neciJmeno)
```

#### Výsledek

```text
Ahoj Honza
Ahoj Jarda
```
