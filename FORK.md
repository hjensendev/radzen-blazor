# Sicra-fork av Radzen Blazor

Dette repoet (https://github.com/hjensendev/radzen-blazor) er en fork av
[radzenhq/radzen-blazor](https://github.com/radzenhq/radzen-blazor).
Forken finnes for å vedlikeholde Sicra-temaet i branchen `sicra`.

## Brancher

| Branch   | Innhold                                                        |
|----------|----------------------------------------------------------------|
| `master` | Speiler `master` hos Radzen. Ikke gjør egne endringer her.     |
| `sicra`  | Radzen `master` + Sicra-temaet. Det er denne som vedlikeholdes. |

## Sicra-temaet

Filene ligger i `Radzen.Blazor/themes/`:

| Fil                  | Beskrivelse                                                        |
|----------------------|--------------------------------------------------------------------|
| `sicra-base.scss`    | Base-tema med Sicra-farger, tilpasset Radzen-versjonen for .NET 10 |
| `sicra.scss`         | Fullt tema med Sicra-farger, tilpasset Radzen-versjonen for .NET 10 |
| `sicra-base-8.scss`  | Opprinnelig base-tema slik det ble bygget for .NET 8               |
| `sicra-8.scss`       | Opprinnelig fullt tema slik det ble bygget for .NET 8              |

I tillegg gjør `components/blazor/_dialog.scss` knappene i dialogfoten om
til `var(--rz-secondary)`.

.NET 10-versjonene skiller seg fra .NET 8-versjonene bare ved at de bruker
`@use 'sass:color';` og `color.mix(...)` i stedet for `mix(...)`, slik
`default-base.scss` gjør i dagens Radzen.

Alle `themes/**/*.scss` kompileres til CSS ved bygg av `Radzen.Blazor`
(target `net10.0`), se `CompileSass` i `Radzen.Blazor/Radzen.Blazor.csproj`.

## Oppsett av remotes

```
origin  git@github.com:radzenhq/radzen-blazor.git   # Radzen (oppstrøms)
fork    git@github.com:hjensendev/radzen-blazor.git # Sicra-forken
```

`sicra` følger `fork/sicra`, så `git push` og `git pull` på `sicra` går
mot forken.

Ved ny klone:

```
git clone git@github.com:hjensendev/radzen-blazor.git
cd radzen-blazor
git remote rename origin fork
git remote add origin git@github.com:radzenhq/radzen-blazor.git
git fetch --all
git switch --track fork/sicra
```

## Hente oppdateringer fra Radzen

```
git switch sicra
git fetch origin
git merge origin/master
git push
```

Etter en merge bør du sjekke om Radzen har endret `default-base.scss` eller
`default.scss`, og i så fall ta med de samme endringene i
`sicra-base.scss` og `sicra.scss`:

```
git diff ORIG_HEAD HEAD -- Radzen.Blazor/themes/default-base.scss Radzen.Blazor/themes/default.scss
```

Test at temaet kompilerer, for eksempel:

```
cd Radzen.Blazor/themes
npx sass --no-source-map sicra-base.scss /tmp/sicra-base.css
npx sass --no-source-map sicra.scss /tmp/sicra.css
```

Advarsler om `@import` er forventet; de kommer også for Radzens egne temaer.

Hvis du vil holde `master` i forken lik Radzen:

```
git push fork origin/master:master
```

## Historikk

- 2025-04-24: Sicra-farger lagt til (`sicra-base.scss`, `sicra.scss`), bygget for .NET 8.
- 2026-10-10: `master` fra Radzen merget inn i `sicra`. .NET 8-temaene kopiert til
  `*-8.scss`, og `sicra-base.scss`/`sicra.scss` oppdatert til `sass:color`.
  Repoet forket til `hjensendev/radzen-blazor`.
