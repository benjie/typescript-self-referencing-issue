# TypeScript 4.5 doesn't support self-referencing

From the Node.js documentation:

> ## Self-referencing a package using its name
>
> Within a package, the values defined in the package’s package.json "exports" field can be referenced via the package’s name. 
>
> -- https://nodejs.org/api/packages.html#self-referencing-a-package-using-its-name

This repo contains two equivalent ESM-flavour modules, one in pure JavaScript
and one in TypeScript using the `nodenext` module config.

`index.js` (or `.ts`) imports `print` from `print.js` and calls it.

`print.js` (or `.ts`) imports a number **from the module itself**:

```ts
import { nr } from "@localrepo/js";
```

In JavaScript this works; but in TypeScript it results in an error (see below).

## In JavaScript

```
cd js
node dist/index.js
```

Result:

```
42
```

## In TypeScript

```
cd ts
yarn
yarn tsc
```

Result:

```
$ <path>/ts/node_modules/.bin/tsc
src/print.ts:1:20 - error TS7016: Could not find a declaration file for module '@localrepo/ts'. '<path>/ts/dist/index.js' implicitly has an 'any' type.

1 import { nr } from "@localrepo/ts";
                     ~~~~~~~~~~~~~~~


Found 1 error.

error Command failed with exit code 2.
```




