
As part of bringing your own code to page designs, you'll likely need to work with some `npm` libraries in addition to the `@kinde/infrastructure` package.

Kinde supports a handful of carefully selected libraries from `npm`. These include:

`@kinde/infrastructure`

This is Kinde’s package that supplies helpers and methods to give you the best possible authoring experience of Kinde pages and workflows.

For server side React templates:

`react` and `react-dom/server.browser`

For Liquid templates:

`liquidjs`

Attempting to import unsupported packages into your runtime code will result in your deployment failing.

Dev dependencies (e.g packages used for linting, tests and build steps) can be used, as they do not affect the Kinde runtime environment. Any code dependent on these packages should be prebuilt before deploying to Kinde.
