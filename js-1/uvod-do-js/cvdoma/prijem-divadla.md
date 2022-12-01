---
title: Příjem divadla
demand: 2
---

1. Jeden lístek do divadla :i[Pěst na oko] stojí **12 euro**. Spočítejte měsíční příjem divadla ze vstupného přichází-li průměrně **174 návštěvníků** na jedno představení a divadlo hraje **15 představení** měsíčně. Uložte výsledek do proměnné `prijem`.
1. Divadlo se rozhodlo prodávat studentské vstupné ve výši **65 %** plného vstupného. Jak se změní měsíční příjem divadla pokud víme, že **40 %** návštěvníků jsou studenti?

---solution

```js
const plnaCena = 12
const pocetNavstevnikuNaPredstaveni = 174
const pocetPredstaveniZaMesic = 15
const prijem =
	plnaCena * pocetNavstevnikuNaPredstaveni * pocetPredstaveniZaMesic
document.body.innerHTML = '<p>Příjem divadla: ' + prijem + ' Kč</p>'

const studentskaCena = 0.65 * plnaCena
const studenti = 0.4
const prijmySeStudenty =
	(plnaCena * (1 - studenti) + studentskaCena * studenti) *
	pocetNavstevnikuNaPredstaveni *
	pocetPredstaveniZaMesic
document.body.innerHTML +=
	'<p>Příjem divadla se slevou pro studenty: ' + prijmySeStudenty + ' Kč</p>'
```
