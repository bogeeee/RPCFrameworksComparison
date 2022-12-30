Project stats:
TODO
## Common
|                                   | [restfuncs](https://github.com/bogeeee/restfuncs) | [telefunc](https://telefunc.com/) | [tRPC](https://trpc.io/) | [wildcard](https://github.com/brillout/wildcard-api) | 
| :-------------------------------- | :---: | :-------------: | :--------------: | :--------------: |  
| Comment | Universal/ frameworkless. Uses server side transformation at build time| Specially tailored for certain frameworks. Uses server side and client side transformers at build time but that limits the use cases.| Widely used, many 3rd party integrations | Precedent of telefunc. Not maintained anymore.
Your service is defined as a | class | module | `t.router({ ...})` declaration | by adding functions to the `server` object
End-to-end-type safety (compile time)| ✅ | ✅  | ✅ | ❌
Automatic arguments validation at runtime  | ✅ | ✅  | ❌ | ❌ |
**Compiles with** (full features assumed)|
tsc / ttsc | ✅ | ❌  | ✅ | ✅
ts-node | <a title="with -C ttypescript">✅*</a> | ❌  | ✅ | ✅
tsx / esbuild | <a title="You can still use tsx in development where you don't need arguments validation.">❌*</a> | ❌ | ✅ | ✅
Compiles / runs when **server code** is build within...|
... Vite/SVELTE KIT | ❌ | ✅ | ✅ | ✅
... Next | ❌ | ✅ | ✅ | ✅ 
... Prisma | ❌ | ✅ | ✅ | ✅
**Server**  |
Standalone | ✅ | ❌ | ✅ | ✅
Express | ✅ | ✅  | ✅ | ✅
Koa | ❌ | ✅  | ✅ | ✅
hapi | ❌ | ❌  | ❌ | ✅
**Client**  |
Vite | ✅ | ✅  | ✅ | ✅
Webpack | ✅ | ✅  | ✅ | ✅
Babel | ✅ | ✅  | ✅ | ✅
Other (non-browser) js clients | ✅ | ❌ | ✅ | ✅
Request batching | ❌ | ❌  | ✅ | ❌
React helpers | ❌ | ❌  | <a href="https://trpc.io/docs/react-query">✅</a> | ❌
**Other**  |
Server-Side rendering | ❌ | ✅ | ✅ | ❌
Subscriptions / WebSockets | ❌ | ❌ | ✅ | ❌
REST API spec | <a title="Planned">❌*</a> | ❌ | ✅ | ❌
Auto. generate OpenAPI/Swagger docs | <a title="Planned">❌*</a> | <a title="Planned">❌*</a> | ❌ | ❌

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