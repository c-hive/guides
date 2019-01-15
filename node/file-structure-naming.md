# File structure and naming

## Casing
Folder and files names use **kebab-case**
- e.g. `dummy-model`

Classes use **UpperCamelCase**
- e.g. `DummyClass`

Functions, variables and any others use **lowerCamelCase**
- e.g. `fetchData`

## Special folders
`configs`
- Configuration files.
- Usually JSON structured files or exported constants/functions.

`app/db`
- Database definitions.
- Schemas, models.

`app/utils`
- Non-component, individual, reusable functions.
- For example API calls, queries and so forth.

## Grouping
- The folder structure inside `app` is grouped by components.
- Each component should contain its related files.

An example to demonstrate the regular structure of a component:

```
.
├── dummy
│   ├── dummy.js # functionalities
│   ├── dummy.routes.js # component related API endpoints
│	  ├── dummy.test.js # test suites
│   ├── index.js # export functionalities
```
