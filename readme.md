# `ultimate-tooling`

`tsconfig.json` can seem extremely scary. But really, it's only 2 or 3 decisions you need to make.

This package makes those decisions even easier. Based on my [TSConfig Cheat Sheet](https://www.totaltypescript.com/tsconfig-cheat-sheet).

## Setup

1. Install:

```bash
npm install --save-dev ultimate-tooling
```

2. Choose which `tsconfig.json` you need from the [list](#list-of-tsconfigs) below.

3. Add it to your `tsconfig.json`:

```jsonc
{
  // I'm building an app that runs in the DOM with an external bundler
  "extends": "ultimate-tooling/bundler/dom"
}
```

## List of TSConfigs

The tricky thing about `tsconfig.json` is there is _not_ a single config file that can work for everyone. But, with two or three questions, we can get there:

### Are You Using `tsc` To Turn Your `.ts` Files Into `.js` Files?

#### Yes

If yes, use this selection of configs in your `tsconfig.json` for typechecking:

```jsonc
{
  // tsconfig.json
  "extends": "ultimate-tooling/tsc/typechecking/dom", // If your code runs in the DOM
  "extends": "ultimate-tooling/tsc/typechecking/no-dom" // If your code doesn't run in the DOM
}
```

and this selection of configs in your `tsconfig.build.json` for build (use script `tsc -p tsconfig.build.json`):

```jsonc
{
  // tsconfig.build.json
  // My code runs in the DOM:
  "extends": "ultimate-tooling/tsc/build/dom/app", // For an app
  "extends": "ultimate-tooling/tsc/build/dom/library", // For a library
  "extends": "ultimate-tooling/tsc/build/dom/library-monorepo", // For a library in a monorepo

  // My code _doesn't_ run in the DOM (for instance, in Node.js):
  "extends": "ultimate-tooling/tsc/build/no-dom/app", // For an app
  "extends": "ultimate-tooling/tsc/build/no-dom/library", // For a library
  "extends": "ultimate-tooling/tsc/build/no-dom/library-monorepo" // For a library in a monorepo
}
```

#### No

If no, you're probably using an external bundler. Most frontend frameworks, like Vite, Remix, Astro, Nuxt, and others, will fall into this category. If so, use this selection of configs:

```jsonc
{
  // My code runs in the DOM:
  "extends": "ultimate-tooling/bundler/dom",

  // My code _doesn't_ run in the DOM (for instance, in Node.js):
  "extends": "ultimate-tooling/bundler/no-dom"
}
```

## Options Not Covered:

### `outDir`

Mostly relevant for when you're transpiling with `tsc`. If you want to change the output directory of your compiled files, you can set the `outDir` option in your `tsconfig.json`:

```json
{
  "extends": "ultimate-tooling/tsc/build/no-dom/library",
  "compilerOptions": {
    "outDir": "dist"
  }
}
```

### Framework-Specific Options

I don't yet cover framework-specific options, like `vite`, `next`, `remix`, etc. With enough persuasion, I might add them in the future.

## Why Not Use `@tsconfig/bases`?

The [`@tsconfig/bases`](https://github.com/tsconfig/bases) package is a great resource for TypeScript configurations. However, I disagree with the idea that there is a single "recommended" configuration that works for everyone.

Also, I wanted a set of `tsconfig.json` files that I controlled so I could use them to keep my [Total TypeScript Course Repos](https://github.com/total-typescript) up to date.

If you're looking for a great TypeScript course, check out [Total TypeScript](https://www.totaltypescript.com/).
