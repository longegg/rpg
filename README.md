# RPG Liste

En side i Vue og Typescript som viser en liste med innhold fra et JSON datasett.

Du kan gjøre følgende operasjoner:
1. Søk på en property
2. Filtrere ulike properties
3. Sortere på dato
4. Mulighet til å lage en ny linje
5. Legg ut på Git* med live side.

*Bonus:*
1. Oppdatere et felt
2. Åpne en linje for å se detaljer

## Getting Started

The project is set up using the latest version of Vue and the official Vue project scaffolding tool `create-vue`.

### Dependencies

- Git: https://git-scm.com/downloads
- Bun: https://bun.sh/docs/installation

### Installation and setup

#### Clone the project

```sh
git clone git@github.com:longegg/rpg.git
```

#### Install packages with Bun

```sh
bun install
```

#### Compile and Hot-Reload for Development

```sh
bun run dev
```

#### Type-Check, Compile and Minify for Production

```sh
bun run build
```

#### Lint with [ESLint](https://eslint.org/)

```sh
bun run lint
```

## Deployment

Deployment is set up using Github Pages. The script below will use the dist directory and push the contents to the branch `gh-pages` on Github. It is then served as a static site on https://longegg.github.io/rpg/. Deployment can be set up using Github Actions.

#### Deploy from local machine

```sh
bun run deploy
```

## Additional Resources

- https://mkay11.medium.com/how-to-deploy-your-vite-vue-3-application-in-github-pages-2023-2b842f50576a
- https://github.com/tschaub/gh-pages