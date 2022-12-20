# Datové kurzy [![quality assurance](https://github.com/Czechitas-podklady-WEB/data-vyuka/actions/workflows/quality-assurance.yml/badge.svg)](https://github.com/Czechitas-podklady-WEB/data-vyuka/actions)

Studijní materiály pro data na kodim.cz

## Formátování

Pro jednotný formát podkladů je v repozitáři konfigurace pro Prettier, který je možný v IDE nastavit jako výchozí formátovač nebo ho lze pouštět ručně přes:

```sh
npm ci
npm run fix
```

## Nasazení na [kodim.cz](https://kodim.cz)

Změny se nasadí automaticky po pushnutí do větve `deploy` například zadáním příkazu:

```sh
git push origin HEAD:deploy
```

## Lokální běh pro vývoj

Spuštěním příkazu:

```sh
docker-compose up
```

a otevřením [http://localhost:3000](http://localhost:3000) v prohlížeči.
