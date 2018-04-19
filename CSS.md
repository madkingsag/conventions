# MAD Kings CSS Conventions

## Linters

Linters statically analyse your code and force you to follow a common styleguide. They can also help find bugs in your code.

In CSS, we use the [stylelint](https://github.com/stylelint/stylelint) linter using the [@foobarhq/stylelint-config](https://github.com/madkingsag/stylelint-config) preset.

You can install stylelint in your project by creating an `.stylelintrc` config file and setting the above as the preset.

```json
{
  "extends": "@foobarhq/stylelint-config"
}
```

We highly recommend you use an IDE plugin to enable live stylelint linting.

We also recommend you set up automatic checks when you commit using [husky](https://www.npmjs.com/package/husky) and [lint-staged](https://github.com/okonet/lint-staged).

In order to do so, first install both as development dependencies then add lint-staged as a pre-commit hook (husky lets us do that by simply defining a `precommit` script in `package.json`).

```json
{
  "scripts": {
    "precommit": "lint-staged"
  }
}
```

You then need to configure lint-staged by creating a `.lintstagedrc` file and setting it up to run stylelint on any staged (s)css file.

```json
{
  "*.css": "stylelint",
  "*.scss": "stylelint --syntax=scss"
}
```

## SCSS

We usually write our CSS in [SCSS](https://sass-lang.com/). We use SCSS over stylus and SASS because SCSS is closer to being a superset of CSS than the other two. We also use it over LESS for the richer feature set.

## PostCSS

Even though we use SCSS primarily, PostCSS is still a part of our build process. We don't use it to build completely new language like SCSS. Instead we use it to maximise compatibility between browsers.

Here is the list of plugins you should usually add to your projects:

- **[postcss-scss](https://github.com/postcss/postcss-scss)**: Makes it possibile for postcss to parse SCSS (you usually need to run postcss *before* SCSS).
- **[postcss-preset-env](https://github.com/jonathantneal/postcss-preset-env)**: Transpiles new features into old syntax, automatically adds prefixes (`-webkit`, `-moz`, ...) for you. Uses *.browserlistrc* TODO add link to browserlistrc
- **[flexbug-fixes](https://github.com/luisrudge/postcss-flexbugs-fixes)**: Fixes inconsistencies in the different flexbox implementations.
- postcss-import
- **[postcss-nth-child-fix](https://www.npmjs.com/package/postcss-nth-child-fix)**: Fixes an android bug
- **[lost-grid](http://lostgrid.org/)**: This is a bit of an exception. If you want to use CSS Grid but need to support older browsers too, you can use lost-grid as a replacement.
- **[postcss-calc](https://github.com/postcss/postcss-calc)**: Optimise `calc()` expressions.

If you find a good PostCSS plugin, don't hesitate to add it to this list.
