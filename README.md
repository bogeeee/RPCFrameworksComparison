# RPC frameworks comparison


## Common
|                                   | [restfuncs](https://github.com/bogeeee/restfuncs) | [telefunc](https://telefunc.com/) | [tRPC](https://trpc.io/) | [wildcard](https://github.com/brillout/wildcard-api) | 
| :-------------------------------- | :---: | :-------------: | :--------------: | :--------------: |  
| Comment | Uses server side transformers. Aimed for a quick jump in / less config.| Specially tailored for certain frameworks. Uses server side and client side transformers.| Widely used, many 3rd party integrations. | Precedent of telefunc. Not maintained anymore.
Your service is defined as a | class | module | `t.router({ ...})` declaration | by adding functions to the `server` object
End-to-end-type safety (compile time)| ✅ | ✅  | ✅ | <a title="not very convenient" href="https://github.com/brillout/wildcard-api#typescript">❌*</a>
Automatic arguments validation at runtime  | ✅ | ✅  | ❌ | ❌ |
Server-Side rendering | <span title="No special support but possible as the server is universal">❌*</span> | ✅ | ✅ | <a title="No special support but possible as the server is universal" href="https://github.com/brillout/wildcard-api#ssr">❌*</a>
Request batching | ❌ | ❌  | ✅ | ❌
Subscriptions / WebSockets | ❌ | ❌ | ✅ | ❌
REST API spec | <span title="Planned">❌*</span> | ❌ | ✅ | ❌
Auto. generate OpenAPI/Swagger docs | <span title="Planned">❌*</span> | <span title="Planned">❌*</span> | <a href="https://github.com/jlalmes/trpc-openapi">✅</a> | ❌
React helpers | ❌ | ❌  | <a href="https://trpc.io/docs/react-query">✅</a> | ❌
Typesafe session / context objects | ✅ | ❌ | ❌ | ❌ |

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

Except on tRPC it's `await client.query.greet("Bob")`