# React / File structure and naming

## Example project structure

```
.
├── COPYRIGHT.txt
├── package.json
├── public
│   ├── assets
│   │   └── image.jpg
│   ├── favicon.ico
│   └── index.html
├── README.md
├── src
│   ├── common_styles
│   │   └── layout.css
│   ├── components
│   │   ├── App
│   │   │   ├── App.css
│   │   │   ├── App.js
│   │   │   ├── App.test.js
│   │   │   └── App.test.js.snap
│   │   └── Header
│   │       ├── Header.css
│   │       ├── Header.js
│   │       ├── Header.test.js
│   │       └── Header.spec.js.snap
│   └── utils
│       ├── testing
│       │   └── TestHelpers.js
│       └── proxy
│           └── GetProxyURL.js
└── webpack.config.js
```

## Casing

- Category folders use **snake_case**
  - e.g. `utils`, `components`
- Component folders, files and class names use **UpperCamelCase**
	- e.g. `LeagueSelector`, `UrlBuilder`
- Variables, functions use **lowerCamelCase**
	- e.g. `fetchLeagues`, `fetchItems`

## Special folders

- `src/components`
- `src/utils`
  - Non-UI / non-component / utility features not tightly linked to the specific project (e.g. `Api`, `CorsProxy`)
- `src/resources`
  - Machine readable, hardcoded data (e.g. `ItemTypes`, json data files)
- `src/common_styles`
  - style files applied to the whole project

## Grouping

- Files are grouped by functionality (components) in folders
- A component folder contains all related code (e.g. js, styles, tests). See also: [Jest conventions](https://jestjs.io/docs/en/configuration.html#testregex-string)
- [Jest snapshot location can be configured in v24+](https://medium.com/c-hive/no-more-snapshots-folders-with-jest-98de26681764)
- Files can be logically groupped into more folders (e.g. `selectors/LeagueSelector` and `selectors/ItemTypeSelector`)
