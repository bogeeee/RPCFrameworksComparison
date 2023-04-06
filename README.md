# RPC frameworks comparison


## Common
|                                   | [Restfuncs](https://github.com/bogeeee/restfuncs) | [Telefunc](https://telefunc.com/) | [tRPC](https://trpc.io/) | [wildcard](https://github.com/brillout/wildcard-api) | gRPC | Deepkit RPC | Blitz RPC | Remult Backend methods | Phero
| :-------------------------------- | :---: | :-------------: | :--------------: | :--------------: | :--------------: | :--------------: | :--------------: | :--------------: |  :--------------: |  
| Comment | Uses server side transformers. Aimed for a quick jump in / less config.| Specially tailored for certain frameworks. Uses server side and client side transformers.| Widely used, many 3rd party integrations. Also has additional higher level execution plans (links / queries / mutations) to reduce the number of calls but that goes beyond this comparison. | Precedent of telefunc. Not maintained anymore. | TODO | TODO | TODO | TODO | TODO
Your service is defined as a | class | module | `t.router({ ...})` declaration | by adding functions to the `server` object
End-to-end-type safety (compile time)| ✅ | ✅  | ✅ | <a title="not very convenient" href="https://github.com/brillout/wildcard-api#typescript">❌*</a>
Function declarations are in  native language style (see <a href="#a-server-function-looks-like">examples</a>)| ✅ | ✅  | ❌ | ✅ |
Automatic arguments validation at runtime _(implies that you don't have to declare or handle a type twice)_ | <a title="Uses the typescript-rtti transformer to build and inspect types" href="https://typescript-rtti.org">✅*</a> | <span title="Server side code transformation is used (called shielding)">✅*</span>  | <a title="By declaring types as ZOD" href="https://github.com/colinhacks/zod">✅*</a> | ❌ |
Server-Side rendering | <span title="No special support but possible as the server is universal">❌*</span> | ✅ | ✅ | <a title="No special support but possible as the server is universal" href="https://github.com/brillout/wildcard-api#ssr">❌*</a>
Request batching | ❌ | ❌  | ✅ | ❌
Subscriptions / WebSockets | ❌ | ❌ | ✅ | ❌
File uploads / WebSockets | ❌ | ❌ | ❌ | ❌
[Superjson](https://www.npmjs.com/package/superjson) support: _proper handling of Date/Map/Set/BigInt_ | ❌ | ❌ | <span title="Needs to be cofigured in as a custom transfomer">✅*</span> | ❌ 
REST API spec | <span title="Planned">❌*</span> | ❌ | ✅ | ❌
Auto. generate OpenAPI/Swagger docs | <span title="Planned">❌*</span> | <span title="Planned">❌*</span> | <a href="https://github.com/jlalmes/trpc-openapi">✅</a> | ❌
React helpers | ❌ | ❌  | <a href="https://trpc.io/docs/react-query">✅</a> | ❌
Typesafe session / context objects | ✅ | ❌ | ✅ | ❌ |

## Compile / run
Assuming full features (like automatic arguments validation)

|                                   | [restfuncs](https://github.com/bogeeee/restfuncs) | [telefunc](https://telefunc.com/) | [tRPC](https://trpc.io/) | [wildcard](https://github.com/brillout/wildcard-api) | 
| :-------------------------------- | :---: | :-------------: | :--------------: | :--------------: |  
tsc / ttsc | ✅ | ❌  | ✅ | ✅
ts-node | <span title="with -C ttypescript">✅*</span> | ❌  | ✅ | ✅
tsx / esbuild | <span title="You can still use tsx in development where you don't need arguments validation.">❌*</span> | ❌ | ✅ | ✅
When **server code** is build within...|
... Vite/SVELTE KIT | ❌ | ✅ | ✅ | ✅
... Next | ❌ | ✅ | ✅ | ✅ 
... Prisma | ❌ | ✅ | ✅ | ✅

## Server frameworks

|                                   | [restfuncs](https://github.com/bogeeee/restfuncs) | [telefunc](https://telefunc.com/) | [tRPC](https://trpc.io/) | [wildcard](https://github.com/brillout/wildcard-api) | 
| :-------------------------------- | :---: | :-------------: | :--------------: | :--------------: | 
Standalone | ✅ | ❌ | ✅ | ✅
Express | ✅ | ✅  | ✅ | ✅
Koa | ❌ | ✅  | ✅ | ✅
hapi | ❌ | ❌  | ❌ | ✅
Electron | ❌ | ❌  | ✅ | ❌ 
µWebSockets.js | ❌ | ❌  | ✅ | ❌
Other |  |  | <a href="https://trpc.io/docs/awesome-trpc#library-adapters">see here</a>|


## Clients / packagers

|                                   | [restfuncs](https://github.com/bogeeee/restfuncs) | [telefunc](https://telefunc.com/) | [tRPC](https://trpc.io/) | [wildcard](https://github.com/brillout/wildcard-api) | 
| :-------------------------------- | :---: | :-------------: | :--------------: | :--------------: | 
**Client**  |
Vite | ✅ | ✅  | ✅ | ✅
Webpack | ✅ | ✅  | ✅ | ✅
Babel | ✅ | ✅  | ✅ | ✅
All **other** packagers and non-browser js clients (that can load NPM packages)| ✅ | ❌ | ✅ | ✅
Special frontend framework utils / other|  |  | <a href="https://trpc.io/docs/awesome-trpc#frontend-frameworks">see here</a>|


## A server function looks like

### restfuncs
````typescript
async greet(name: string) {
    return `Hello ${name} from the server`
}
````

### telefunc
````typescript
export async function greet(name: string) {
    return `Hello ${name} from the server`
}
````

### tRPC
````typescript
  greet: t.procedure    
    .input(z.object({ name: z.string() })) // arguments validation
    .query(async ({ name }) => {
        return `Hello ${name} from the server`
    })
````

### wildcard
````typescript
server.greet = async function(name: string) {    
    
    // ... arguments validation code
    
    return `Hello ${name} from the server`
}
````

<br/><br/><br/>

## And on the client, this looks like (in case you're new to RPC ;):

````typescript
console.log(await client.greet("Bob"))
````

Except on tRPC it's `await client.greet.query("Bob")`