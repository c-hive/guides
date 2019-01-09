# File structure

#### The folder structure is based on the **MVC** architecture.

- ***M**odel*:
	- Database definitions, e.g. schemas and models.
- ***V**iew*:
	- Consists of view templates, such as `.ejs` or `.html`.
	- The appropriate one of the templates will be represented to the user when an endpoint is being visited.
- ***C**ontroller*: 
	- Communicates between the model and the view.
	- Specific functions for individual routes. 

#### Additional folders
- *configs*: 
	- Configuration files.
	- Usually variables, JSON files and so forth.
	- For example `EnvironmentVariables.js`, that provides the essential variables for the initialization process.
- *routes*:
	- *Express* routes.
	- Project's API endpoints definition place.
	- Running the corresponding controller's function.
- *utils*:
	- Utility functions.
	- Anything that can be decoupled.
	- For example API calls, initializations and so forth.

#### Grouping

- Each folder should contain its related files. It usually consists of a JavaScript file along with the associated test suites separated into an individual file.
- Files can be logically grouped into more folders(e.g. `routes/ExternalAPIRoutes`).

