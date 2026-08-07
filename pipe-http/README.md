# pipe-http

HTTP client module for Pipe — all HTTP methods with custom headers, auth helpers, and JSON support.

Requires Pipe v0.8+ (`http_request` builtin).

## Functions

- `hget(url, headers)` — HTTP GET request
- `hpost(url, body, headers)` — HTTP POST request
- `hput(url, body, headers)` — HTTP PUT request
- `hdelete(url, headers)` — HTTP DELETE request
- `hget_json(url, headers)` — GET + parse response as JSON
- `hpost_json(url, body, headers)` — POST + parse response as JSON
- `req(method, url, headers, body)` — Generic request with any HTTP method
- `auth_bearer(token)` — Create Bearer auth header map
- `auth_basic(user, pass)` — Create Basic auth header map
- `auth_apikey(key)` — Create API key header map
- `is_ok(resp)` — Check if response status is 2xx
- `is_error(resp)` — Check if response status is 4xx/5xx

All methods return a response map: `{status: int, headers: map, body: string}`

## Usage

```pipe
import "https://raw.githubusercontent.com/MachuraHarry/pipe-modules/master/pipe-http/module.pipe"

-- Simple GET
r: hget "https://api.example.com/data" {}
print (get r "body")

-- GET with Bearer auth
auth: auth_bearer "mytoken123"
r2: hget "https://api.example.com/secure" auth

-- POST JSON
h: {}
set h "Content-Type" "application/json"
r3: hpost "https://api.example.com/data" "{\"name\":\"test\"}" h

-- Parse JSON response directly
data: hget_json "https://api.example.com/data" {}
print (get data "key")

-- Check status
if is_ok r
    print "Success!"
```
