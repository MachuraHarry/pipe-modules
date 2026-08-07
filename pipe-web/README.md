# pipe-web

Web framework for Pipe — ASP.NET / Express style routing, middleware, and response helpers.

Requires Pipe v0.8+ (`http_server`, `http_close` builtins). Runs in Tree-Walker mode only.

## Install

```bash
pipe -get pipe-web
```

## Quick Start

```pipe
import "pipe-web"

fn hello req
    params: get req "params"
    name: get params "name"
    ok {message: "Hello " ++ name ++ " from Pipe!"}

app: app "Demo"
route_get app "/hello/:name" hello
serve app "0.0.0.0:8080" 60000
```

## API

### Application

- `app(name)` — Create a new application with routes and middleware containers.

### Routing

- `route_get(app, path, handler)` — Register a GET route
- `post(app, path, handler)` — Register a POST route
- `put(app, path, handler)` — Register a PUT route
- `delete(app, path, handler)` — Register a DELETE route
- `patch(app, path, handler)` — Register a PATCH route
- `any(app, path, handler)` — Register for all HTTP methods

**Path patterns:**
- `/users/:id` — captures `:id` as path parameter (accessible via `get req "params"`)
- `/static/*` — wildcard captures remaining path segments
- `/about` — exact match

**Handler signature:** `fn(req)` receives request map `{method, path, query, headers, body, params}` and returns a response map `{status, headers, body}`.

### Middleware

- `use(app, middleware)` — Register middleware function `fn(req)`.
  - Return a response map to short-circuit the request (e.g. auth failure → 401).
  - Return `nil` to continue to next middleware or route handler.
  - Modify `req` in place via `set` to enrich the request.

### Server

- `listen(app, addr)` — Start HTTP server on addr (non-blocking), returns server handle.
- `serve(app, addr, ms)` — Convenience: listen + sleep ms + close. For demos and simple scripts.
- `close(server)` — Shut down the server.

### Response Helpers

- `json(status, data)` — JSON response
- `ok(data)` — 200 JSON shortcut
- `text(status, str)` — Plain text response
- `html(status, str)` — HTML response
- `redirect(status, url)` — Redirect with Location header
- `not_found(msg?)` — 404 response (default: "Not Found")
- `internal(msg?)` — 500 response (default: "Internal Server Error")
- `json_body(req)` — Parse `req.body` as JSON

## Full Example

```pipe
import "pipe-web"

-- Auth middleware
fn auth req
    headers: get req "headers"
    token: get headers "Authorization"
    if token == nil
        return json 401 {error: "Missing token"}
    -- Pass through (optional: set user info on req)
    nil

-- Handlers
fn home req
    html 200 "<h1>Welcome to Pipe Web</h1><p>Try <code>/api/data</code></p>"

fn api_data req
    ok {server: "pipe-web", version: "1.0.0", time: now}

fn user_detail req
    params: get req "params"
    id: get params "id"
    ok {user_id: id, name: "User " ++ id}

fn create_item req
    body: json_body req
    json 201 {received: body, created: true}

-- App setup
api: app "MyAPI"
use api auth
route_get api "/" home
route_get api "/api/data" api_data
route_get api "/users/:id" user_detail
post api "/items" create_item

serve api "0.0.0.0:8080" 300000
```

## Limitations

- **Tree-Walker mode only** — handler functions must run in tree-walker mode (no `-vm` flag). This is the same limitation as the `go` builtin.
- **Sequential request processing** — requests are handled one at a time (mutex in http_server). Parallel AI calls within a handler via `>>` or `ai_batch` work fine.
- **One app per process** — the module uses a global variable for the current app. Running multiple apps in the same process is not supported.
- **Route count** — recursive route matching is tail-call optimized. Practical for < 100 routes.

## License

MIT
