# @nrwl/js inline.ts `postProcessInlinedDependencies` tries to move the same inline library twice

## The issue

If you want to export non buildable libraries inside a publishable libraries, the `postProcessInlinedDependencies` tries to move the same library multiple times.

As the library has already moved the first time, the 2nd time will fail and throw an error.

This is caused by the following line: https://github.com/nrwl/nx/blob/6f747f9a5cf125e9b1b5ae3570b773e49e4c1f54/packages/js/src/utils/inline.ts#L82

## How to reproduce

```bash
$ npm i
$ npx nx build website
```

## Logs

Log of `Object.values(inlineGraph.dependencies)`:

```
[
  [
    'page-website',
    'article-website',
    'navigation-website',
    'richtext-website',
    'membership-website',
    'block-content-website',
    'authentication-website',
    'website-builder',
    'website-api',
    'website-api-v2'
  ],
  [ 'website-api', 'website-builder', 'block-content-website' ],
  [
    'website-api', 'website-api',
    'website-api', 'website-api',
    'website-api', 'website-api',
    'website-api', 'website-api',
    'website-api', 'website-api',
    'website-api', 'website-api'
  ],
  [
    'website-builder',
    'website-api',
    'richtext-website',
    'website-builder',
    'website-api',
    'richtext-website',
    'website-builder',
    'website-api',
    'richtext-website'
  ],
  [
    'website-builder',
    'website-builder',
    'website-builder',
    'website-builder'
  ],
  [ 'website-api', 'website-builder', 'block-content-website' ],
  [ 'website-api', 'website-builder' ],
  [ 'website-builder', 'website-api', 'authentication-website' ],
  [ 'website-api', 'website-api' ]
]
```

Log of `depOutputPath, destDepOutputPath`:

```
/Users/itrulia/Documents/nrwl-js-postProcessInlinedDependencies-issue/dist/libs/website libs/page/website
/Users/itrulia/Documents/nrwl-js-postProcessInlinedDependencies-issue/dist/libs/website libs/article/website
/Users/itrulia/Documents/nrwl-js-postProcessInlinedDependencies-issue/dist/libs/website libs/navigation/website
/Users/itrulia/Documents/nrwl-js-postProcessInlinedDependencies-issue/dist/libs/website libs/richtext/website
/Users/itrulia/Documents/nrwl-js-postProcessInlinedDependencies-issue/dist/libs/website libs/membership/website
/Users/itrulia/Documents/nrwl-js-postProcessInlinedDependencies-issue/dist/libs/website libs/block-content/website
/Users/itrulia/Documents/nrwl-js-postProcessInlinedDependencies-issue/dist/libs/website libs/authentication/website
/Users/itrulia/Documents/nrwl-js-postProcessInlinedDependencies-issue/dist/libs/website libs/website-builder
/Users/itrulia/Documents/nrwl-js-postProcessInlinedDependencies-issue/dist/libs/website libs/website-api
/Users/itrulia/Documents/nrwl-js-postProcessInlinedDependencies-issue/dist/libs/website libs/website-api-v2
/Users/itrulia/Documents/nrwl-js-postProcessInlinedDependencies-issue/dist/libs/website libs/website-api
```
