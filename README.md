Reproduces an issue with TypeScript isolatedModules. 

When a type and a const have the same name, isolatedModules will think the type is being re-exported.


```ts
// Bar.ts
type Bar = 'Bar';
export const Bar = 'Bar';
export default Bar;
// index.ts
export { default } from './Bar' // Fails typecheck
```

To reproduce, clone the repo and run:

```
$ npm i
$ npm run typecheck

> ts-isolated-modules-weirdness@1.0.0 typecheck
> tsc --noEmit

src/index.ts:1:10 - error TS1205: Re-exporting a type when 'isolatedModules' is enabled requires using 'export type'.

1 export { default } from './Bar'
           ~~~~~~~


Found 1 error in src/index.ts:1
```

Notes:
* If you add `export type Bar` in Bar.ts, typecheck passes.
* If you remove `export` from `export const Bar`, typecheck passes.