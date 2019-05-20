# Node.js / File structure and naming

- The [Express generator](http://expressjs.com/en/starter/generator.html) creates a familiar skeleton.
- There's [no consensus](https://stackoverflow.com/a/47945694/2771889) on the right structure (e.g. [1](https://www.infoworld.com/article/3204205/node-js/7-keys-to-structuring-your-nodejs-app.html), [2](https://blog.risingstack.com/node-hero-node-js-project-structure-tutorial/)) but it doesn't matter from the framework's point of view.
- The project is structured as described [here](https://blog.risingstack.com/node-hero-node-js-project-structure-tutorial/). The main idea is similar to [Angular's generator](https://github.com/angular/angular-cli#generating-components-directives-pipes-and-services): organize around features, not roles. Additionally:
  - business logic is stored in an `app` folder
  - the `routes` folder is removed from the project root and instead routes are defined in their corresponding components


## Example project structure

```
.
├── app
│   ├── errors
│   │   └── record-already-exists-error.js
│   ├── db
│   │   └── user
│   │       ├── user.js
│   │       └── user.test.js
│   ├── payments
│   │   ├── payments.js
│   │   ├── payments.routes.js
│   │   ├── payments.test.js
│   │   └── index.js
│   ├── users
│   │   ├── users.js
│   │   ├── users.routes.js
│   │   ├── users.test.js
│   │   └── index.js
│   └── utils
│       └─── response-parser
│            ├── response-parser.js
│            ├── response-parser.test.js
│            └── index.js
├── app.js
├── config
│   └── external-service-config.js
├── doc
│   └── mvp-proposal.md
├── COPYRIGHT.txt
├── package.json
├── README.md
```

## Casing
Folder and files names use **kebab-case**
- e.g. `dummy-model`

Classes use **UpperCamelCase**
- e.g. `DummyClass`

Functions, variables and any others use **lowerCamelCase**
- e.g. `fetchData`

## Special folders
`config`
- Configuration files.
- Usually JSON structured files or exported constants/functions.

`app/db`
- Database definitions.
- Schemas, models.

`app/utils`
- Reusable, generic purpose functions.

## Grouping
- Files are grouped by functionality (components) in folders.
- Each component should contain its related files.
- Files can be logically groupped into more folders (e.g. `external/users` and `internal/users`).
