# Bun & Node & Deno Benchmark Result

## use tools

- https://github.com/codesenberg/bombardier

## result

- Time per request：1回のリクエストにかかった時間
- Requests per second：1秒間に処理できたリクエスト数

```shell
$ ab -k -c 10 -n 10000 http://127.0.0.1:3000/
```

| ランタイム | Time per request | Requests per second     |
|-------|------------------|-------------------------|
| Node  | 0.356 [ms]       | 28068.84 [#/sec] (mean) |
| Deno  | 0.154 [ms]       | 65106.29 [#/sec] (mean) |
| Bun   | 0.258 [ms]       | 38747.98 [#/sec] (mean) |

## test code

### bun

```shell
$ bun init
```

```typescript
export default {
    port: 3000,
    fetch(request: Request) {
        return new Response("Hello World");
    }
};
```

### node

```typescript
const http = require("http");
const port = 3000;

const server = http.createServer((request, response) => {
    response.writeHead(200, {
        "Content-Type": "text/html"
    });
    const responseMessage = "<h1>Hello World</h1>";
    response.end(responseMessage);
});

server.listen(port);
```

### deno

```shell
$ brew install deno
```

```typescript
const server = Deno.listen({port: 3000});

for await (const conn of server) {
    serveHttp(conn);
}

async function serveHttp(conn: Deno.Conn) {
    const httpConn = Deno.serveHttp(conn);
    for await (const requestEvent of httpConn) {
        const body = "<h1>Hello World</h1>";
        requestEvent.respondWith(
            new Response(body, {
                status: 200
            })
        );
    }
}
```