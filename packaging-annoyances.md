The ultimate nodejs / web / typescript / modules / commonjs pitfall collection.   
Put all my anger into one table!!!  

ok, not quiet finished yet...;)

## When publishing a library
_Assumed, i wanted to publish for nodejs / web / modules / commonjs. With typescript types and source maps_

|                                   | Pro | Con |
| :-------------------------------- | :-----------------: | :-------------: |  
| Setting `type="commonjs"` | | Some dependencies may be built for module only = cannot properly use them.|
| Setting `type="module"` | Modulejs code works / has no trouble with importing local modules (see below)| Commonjs project can use package only via `require(...)`, but that makes typescript types unusabe. |
| Dual build: `type="commonjs"`. Dist files in dist/commonjs/\*.js + dist/esm/\*.js) | | Tsx or tsc+node: `SyntaxError: The requested module 'myPublishedPackage' does not provide an export named 'MySymbol'`. Bun works|
| Dual build: `type="commonjs"`. Dist files in dist/commonjs/\*.js + dist/esm/\*.mjs) | | - Tsx or tsc+node: `Error [ERR_MODULE_NOT_FOUND]: Cannot find module '/.../node_modules/myPublishedPackage/dist/esm/MyInternalSymbol' imported from /.../node_modules/myPublishedPackage/dist/esm/index.mjs`<br/> - Failing to run with **vite devserver**: dist/esm/\*.js files are not recognized because of wrong file extension. These files would need to be renamed + also all internal imports (uff) to make it work. vite static build works. Bun works.|
| Dual build: `type="module"`. Dist files in dist/commonjs/\*.js + dist/esm/\*.js) | | When used in a commonjs project and compiling with Tsc: error TS1479: The current file is a CommonJS module whose imports will produce 'require' calls; however, the referenced file is an ECMAScript module and cannot be imported with 'require'. Consider writing a dynamic 'import("myPublishedPackage")' Seems to be the same like "Single build: type="module", see below. So this version makes no sense. Tsx, Bun, and Vite-devserver work| 
| Single build: `type="commonjs"` + esm wrapper | Avoids [dual package hazard](https://nodejs.org/api/packages.html#approach-1-use-an-es-module-wrapper) | - Needs a (hand written) esm wrapper: index_esm.mjs <br/> - Still we know that the current way can cause problems (when enabling strict=true. Not investigated which one of the strict options it really is), cause there is no .d.ts file for **this file**. But in the wild it seems fine somehow and all types are found:) <br/> - Error in vite build and vite devserver: RollupError: `"default" is not exported by ".../dist/commonjs/index.js", imported by ".../index_esm.mjs"`|
| Single build: `type="module"` | | For commonjs, you need a top level await + `await import("myPublishedPackage")` to use the package (with types). Or apply some hacks with `import type` and cast to the type then |
| Import local modules without extension |
| Import local modules via .js extension |

Extra: In both cases (commonjs/module), you can hit dependencies that work only in one variant. At compile time or at publishing time (hurray!).  

## When consuming a library

|                                   | Pro | Con |
| :-------------------------------- | :-----------------: | :-------------: |  
|  
