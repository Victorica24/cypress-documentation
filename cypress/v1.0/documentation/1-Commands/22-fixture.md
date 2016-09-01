slug: fixture
excerpt: Load a fixture to represent data

[block:callout]
{
  "type": "info",
  "body": "[Read about Creating Fixtures first.](https://on.cypress.io/guides/creating-fixtures)",
  "title": "New to Cypess?"
}
[/block]

Loads a single fixture file. Image fixtures will be sent as `base64`.

If an extension is omitted, Cypress will attempt to resolve the fixture by order of these extensions:

* `.json`
* `.js`
* `.coffee`
* `.html`
* `.txt`
* `.csv`
* `.png`
* `.jpg`
* `.jpeg`
* `.gif`
* `.tif`
* `.tiff`
* `.zip`

| | |
|--- | --- |
| **Returns** | the contents of the file, formatted by file extension |
| **Timeout** | `cy.fixture` will wait up for the duration of [`responseTimeout`](https://on.cypress.io/guides/configuration#section-timeouts) for the server to process this command. |

***

# [cy.fixture( *fixture* )](#section-single-fixture-usage)

Loads the fixture at the specified filepath within `cypress/fixtures`.

***

# [cy.fixture( *fixture*, *encoding* )](#section-encoding)

Loads the fixture at the specified filepath within `cypress/fixtures`, using the encoding specified when reading the file.

***

# Options

Pass in an options object to change the default behavior of `cy.fixture`.

**[cy.fixture( *fixture*, *options* )](#options-usage)**

**[cy.fixture( *fixture*, *encoding*, *options* )](#options-usage)**

Option | Default | Notes
--- | --- | ---
`timeout` | [`responseTimeout`](https://on.cypress.io/guides/configuration#section-timeouts) | Total time to wait for the `cy.fixture` command to be processed

***

# Single Fixture Usage

## Load the `users.json` fixture

```javascript
cy.fixture("users.json")
```

***

## Omit the fixture file's extension

```javascript
cy.fixture("admin")
```

Cypress will search for files called `admin` and resolve the first one in this order:

1. `{your project root}/cypress/fixtures/admin.json`
1. `{your project root}/cypress/fixtures/admin.js`
1. `{your project root}/cypress/fixtures/admin.coffee`
1. `{your project root}/cypress/fixtures/admin.html`
1. `{your project root}/cypress/fixtures/admin.txt`
1. `{your project root}/cypress/fixtures/admin.csv`
1. `{your project root}/cypress/fixtures/admin.png`
1. `{your project root}/cypress/fixtures/admin.jpg`
1. `{your project root}/cypress/fixtures/admin.jpeg`
1. `{your project root}/cypress/fixtures/admin.gif`
1. `{your project root}/cypress/fixtures/admin.tif`
1. `{your project root}/cypress/fixtures/admin.tiff`
1. `{your project root}/cypress/fixtures/admin.zip`
***

## Image fixtures will be sent as `base64`

```javascript
cy.fixture("images/logo.png").then(function(logo){
  // logo will be encoded as base64
  // and should look something like this:
  // aIJKnwxydrB10NVWqhlmmC+ZiWs7otHotSAAAOw==...
})
```

***

# Notes

## Nesting

You can nest fixtures within folders and reference them by defining the path to the file:

`{your project root}/cypress/fixtures/users/admin.json`

```javascript
cy.fixture("users/admin.json")
```

***

## Validation

Cypress will automatically validate your fixtures. If your `.json`, `.js`, or `.coffee`  files contain syntax errors, they will automatically be shown in the Command Log.

***

## Formatting

Cypress automatically formats your fixture files. That means you can paste in a single line of `json` and the next time Cypress serves this fixture, it will format / indent the `json` which makes it easier to read and debug.

***

## Encoding

Cypress automatically determines the encoding for the following file types:

* `.json`
* `.js`
* `.coffee`
* `.html`
* `.txt`
* `.csv`
* `.png`
* `.jpg`
* `.jpeg`
* `.gif`
* `.tif`
* `.tiff`
* `.zip`

For other types of files, they will be read as "utf8" by default. You can specify a different encoding by passing it as the second argument.

```javascript
cy.fixture("foo.bmp", "base64")
```

The following encodings are supported:

* `ascii`
* `base64`
* `binary`
* `hex`
* `latin1`
* `utf8`
* `utf-8`
* `ucs2`
* `ucs-2`
* `utf16le`
* `utf-16le`

***

# Usage with `cy.route()`

Fixtures can be referenced directly by the special keywords: `fixture:` or `fx:`.

This enables you to set a fixture as the response for a route without having to first use the `cy.fixture` command.

## Example 1:

```javascript
cy.route("GET", /users/, "fixture:users") // this works
cy.route("GET", /users/, "fx:users")      // this also works
```

This saves you from having to explicitly load the fixture first (like in Example #2).

## Example 2:

```javascript
cy
  .fixture("users").then(function(json){
    cy.route("GET", /users/, json)
  })
```

However if you still need access to the fixture data, instead of yielding the fixture's data in Example #2, we can make use of [aliasing](https://on.cypress.io/guides/using-aliases).

## Example 3:

```javascript
cy
  .fixture("users").as("usersJSON")
  .route("GET", /users/, "@usersJSON")

  ...later on...

  .then(function(){
    // we have access to this.usersJSON since it was aliased
    this.usersJSON
  })
```

Using an alias provides the benefit of terseness and readability, yet still allows you access to the aliased object later on for direct manipulation.

This is useful when asserting about values in the fixture object, or perhaps if you need to change its values prior to handing it off to a [`cy.route`](https://on.cypress.io/api/route).

***

# Command Log

## `fixture` does *not* log in the command log

***

# Related

- [route](https://on.cypress.io/api/route).
- [Creating Fixtures](https://on.cypress.io/guides/creating-fixtures).