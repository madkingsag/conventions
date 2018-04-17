# MAD Kings JavaScript Conventions

## Linters

Linters statically analyse your code and force you to follow a common styleguide. The can also help find bugs in your code.

In JavaScript, we use the [eslint](https://eslint.org) linter using the [@foobarhq/eslint-config](https://github.com/madkingsag/eslint-config) preset.

You can install eslint in your project by creating an `.eslintrc` config file and setting the above as the preset.

```json
{
  "extends": "@foobarhq/eslint-config"
}
```

We highly recommend you use an IDE plugin to enable live eslint linting.

We also recommend you set up automatic checks when you commit using [husky](https://www.npmjs.com/package/husky) and [lint-staged](https://github.com/okonet/lint-staged).

In order to do so, first install both as development dependencies then add lint-staged as a pre-commit hook (husky lets us do that by simply defining a `precommit` script in `package.json`).

```json
{
  "scripts": {
    "precommit": "lint-staged"
  }
}
```

You then need to configure lint-staged by creating a `.lintstagedrc` file and setting it up to run eslint on any staged js file.

```json
{
  "*.js": "eslint",
}
```

## Flow

The [Flow](https://flow.org/) JavaScript Type Checker helps build a stronger codebase by bringing optional static types to JavaScript.

Using it will enable your IDE to provide better autocompletion and will statically detect bugs in your code.

If you need to, here is [How to install flow](https://flow.org/en/docs/install/).

We highly recommend you use an IDE plugin to enable live flow linting.

*Note: Flow is sometimes a bit instable, so we don't block commits based on issues reported by flow yet.*

## `package.json`

### Non-library Projects

For projects that are not libraries and make little sense to publish on NPM, you should do the following:

- Set the `private` property to `true`
- Follow the following structure for the project name: `@madkings/[<client-name>-]<project-name>`.  
 Note: `client-name` is optional for internal projects.
- When releasing a new version, use the date of the release in `YYYY.MM.DD[-num]` format as the new version id (eg. `2018.04.12`). If more than one release happens on the same day (such as for an emergency fix) append a letter incremented in alphanumerical order (eg. `2018.04.12-b`).

Example:

```json
{
  "name": "@madkings/mmc-api",
  "private": "true",
  "version": "2017.11.02-c"
}
```

### Library Projects

- by semver for everything else http://semver.org/

### Commands

NPM lets you define custom scripts in the `package.json` file that can be run using `npm run <script-name>`. At MAD Kings, we have standardised a series of scripts and their expected behavior:

- `start`: Should launch the project as it would be in production (eg. start API, serve app, etc...).
- `start:dev`: Should launch the project in development. Along with it, it should launch builders so the app is continuously built on change. Ideally the app should be automatically refreshed too (hot reloading, nodemon, etc...).
- `build`: Should build the project for production, but not run it.
- `build:watch`: Should build the project continuously on change.
- `prepublishOnly`: *For libraries*. this is a standard NPM command that is run during `npm publish`. It should at the very least run `npm run build`.

### Tool Configuration

As much as possible, avoid putting your tool configuration (eslint, babel, etc...) inside `package.json`. Instead, use their dedicated configuration file (.eslintrc, .babelrc, etc...).

## Standard library

At some point during development, you are going to need to use an external library. During our time at MAD Kings we have curated a list of "good" JavaScript libraries. If you need that kind of functionality, consider using

| Type         | Preferred | Ill-favored    | Reason                                                          |
|--:-:---------|--:-:------|--:-:-----------|-----------------------------------------------------------------|
| DateTime     | Date-fns  | Moment, Luxon  | Date-fns is cherry pickable                                     |
| General Util | lodash    | underscore     | lodash respects semver                                          |
| ES Polyfills | core-js   | Babel-polyfill | BP is just a wrapper around core-js, core-js is cherry-pickable |
| General Util decorators | lodash-decorators | core-decorators, decko |

Although as much as possible use *standards* (DOM, Ecma, etc...) and polyfills when such an alternative is available as this will lead to fewer dependencies and smaller bundle sizes.

Examples:

- Instead of using `left-pad(string)`, use `string.padStart()` and import a `padStart` polyfill from `core-js`. MDN has an extensive list of what new native methods are available.
- Instead of using `jQuery.ajax`, use `fetch`
- Instead of jQuery DOM, use [HTML5 polyfills](https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-Browser-Polyfills)

## Babel

We write our code in the latest version of EcmaScript. If it's [in the spec](https://tc39.github.io/ecma262/) it can be used (we don't actually expect anyone to read the spec don't worry).

In order to be able to run our code in older environments (IE11, Chrome 49, Older safari versions), we use [Babel](https://babeljs.io/) to transpile the new syntax into an ES5 compatible one.

You should use `babel-preset-env` to transpile these new features based on which browser the project supports. (TODO link to browserlistrc). We however allow three exceptions to the rule of "only stable features":
- [Decorators](https://github.com/tc39/proposal-decorators) - [babel plugin](https://www.npmjs.com/package/babel-plugin-transform-decorators)
- [Static class fields](https://github.com/tc39/proposal-static-class-features/) / [**public** class fields](https://github.com/tc39/proposal-class-fields) - [babel plugin](https://www.npmjs.com/package/babel-plugin-transform-class-properties)
- [JSX](https://facebook.github.io/jsx/), usually for React projects.
