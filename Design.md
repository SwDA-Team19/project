# Software Design – Express.js
## 1. Dependencies

### 1.1 Method and Tools

Code dependencies were analysed by inspecting `require()` calls in each file under `lib/`. Because Express is a CommonJS module without a bundler, every `require` at module scope represents a hard compile-time dependency edge. The analysis was performed manually and with `grep`, producing a directed dependency graph.

Knowledge dependencies (co-change) were approximated by examining the project's `History.md` changelog, which records which files are typically modified together across releases.

### 1.2 Code Dependency Graph

```
express.js (entry)
 ├── application.js
 │    ├── view.js          (only internal–internal edge besides utils)
 │    └── utils.js
 ├── request.js            (no internal deps)
 ├── response.js
 │    └── utils.js
 └── [router – external npm package]
```

**External dependencies per module:**

| Module | External deps | Internal deps |
|---|---|---|
| `response.js` | 12 (content-disposition, http-errors, depd, encodeurl, escape-html, on-finished, mime-types, statuses, cookie-signature, cookie, send, vary) | utils.js |
| `application.js` | 5 (finalhandler, debug, once, router, node:http/path) | view.js, utils.js |
| `request.js` | 7 (accepts, type-is, fresh, range-parser, parseurl, proxy-addr, node:net/http) | — |
| `utils.js` | 6 (content-type, etag, mime-types, proxy-addr, qs, node:querystring/buffer/http) | — |
| `view.js` | 2 (debug, node:path/fs) | — |
| `express.js` | 4 (body-parser, merge-descriptors, node:events, serve-static) | application.js, request.js, response.js |

**Which files have most / fewest dependencies and why?**

`response.js` is by far the heaviest module (12 external deps, 1 047 lines). This is expected: HTTP responses involve content negotiation, cookie signing, streaming, MIME type resolution, status codes, and encoding utilities. Each concern is delegated to a focused npm package, keeping the internal logic readable at the cost of a wide fan-out. The many small packages also reflect the "small modules" philosophy of the Node.js ecosystem.

`view.js` has the fewest dependencies (only `debug` and Node built-ins). Its sole responsibility is file-system lookup and delegating rendering to an engine callback, so it has no need for third-party libraries.

`utils.js` has no internal dependencies at all, making it a pure utility leaf — a healthy design choice that avoids circular imports.

### 1.3 Knowledge Dependencies (Co-change)

Examining `History.md` across Express 5.x milestones reveals the following co-change clusters:

| Files often changed together | Reason |
|---|---|
| `response.js` + `utils.js` | New response helpers always require new utility functions (e.g., `setCharset`, `normalizeType`) |
| `application.js` + `express.js` | Changes to the factory or app lifecycle affect both the entry point and the app prototype |
| `request.js` + `response.js` | Content-negotiation changes (e.g., `res.format`, `req.accepts`) span both |
| `view.js` + `application.js` | Changes to engine registration or render caching touch both |

**Inconsistencies with code dependencies:**

- `request.js` and `response.js` have no direct *code* dependency on each other (neither `require`s the other), yet they co-change frequently. This is a knowledge coupling driven by the HTTP protocol: a change to how the request body is parsed often requires a corresponding change to response helpers.
- `view.js` and `express.js` are not directly linked in the code graph, but they co-change whenever the view resolution algorithm is updated (since `express.js` sets the default `View` class).

These inconsistencies are not design flaws; they reflect protocol-level cohesion that is difficult to capture through static import analysis alone. In practice, this means that a team maintaining Express should treat `request.js` and `response.js` as a logical unit for the purposes of code review and testing — a change to one almost always warrants checking the other, even though the import graph gives no indication of this. Similarly, `view.js` and `express.js` should be reviewed together whenever the view resolution algorithm is touched, despite having no direct `require` relationship.

---

## 2. Design Patterns

### 2.1 Factory (Creational) — `createApplication()` in `lib/express.js`

Express does not expose a class. When you call `require('express')`, you get back a function, and calling that function gives you a fully wired application object. This is the Factory pattern, and it is the first design decision a developer encounters when using the framework.

**Description:** The exported module entry point is not a class but a factory function `createApplication()`. Calling `require('express')` returns this function; calling it returns a fully initialised `app` object.

**Participants:**
- *Creator*: `createApplication()` (`lib/express.js`, line 36)
- *Product*: the `app` function object, mixed with `EventEmitter.prototype` and `application` prototype via `merge-descriptors`

**Code link:** https://github.com/expressjs/express/blob/master/lib/express.js#L36-L56

**Problem solved:** Node.js applications need a single, stateful HTTP handler object. A plain constructor (`new Express()`) would work but would break the CommonJS idiom of calling `require('express')()` directly. The factory hides object wiring (mixin of two prototypes + initialisation) behind a single call, and allows the framework to set up `app.request` and `app.response` as prototype-derived objects in one place.

**Alternative:** A class-based approach (`class Application extends EventEmitter`) would be more idiomatic in modern JS. Pros: explicit inheritance, better type inference, cleaner `instanceof` checks. Cons: would break the long-established `express()` call idiom used by millions of applications; mixing `EventEmitter` and routing behaviour into one class would also violate SRP.

---

### 2.2 Chain of Responsibility (Behavioural) — Middleware Stack via `router`

The middleware pipeline is the defining feature of Express — it is what the entire programming model is built around. Under the hood, it is a textbook Chain of Responsibility: a linear sequence of handler objects where each one decides whether to handle the request or pass it to the next.

**Description:** Every call to `app.use()`, `app.get()`, etc. pushes a `Layer` object onto an ordered stack inside the `Router`. When a request arrives, the router iterates the stack calling each layer's `handle()` method and passing a `next` callback. A layer either terminates the cycle (by sending a response) or calls `next()` to forward control to the next layer.

**Participants:**
- *Handler interface*: middleware functions with signature `(req, res, next)`
- *Concrete Handlers*: any function registered via `app.use()` or a route verb
- *Chain manager*: `Router` (external `router` package, delegated by `application.js` line 222)
- *Client*: the incoming HTTP request dispatched by Node's `http.Server`

**Code link:** https://github.com/expressjs/express/blob/master/lib/application.js#L181-L242

**Problem solved:** HTTP request processing is inherently cross-cutting (logging, authentication, body parsing, business logic, error handling all need to run in sequence). The chain pattern allows each concern to be developed independently and composed at startup time, without any single module needing to know about the others.

**Alternative:** A pipeline DSL or event-based dispatcher (like Koa's `async/await` compose or Fastify's hooks) would provide the same composability with better async error handling. Koa's approach eliminates the need for the `next` callback by using `async` generators. The trade-off is a steeper learning curve and incompatibility with the vast ecosystem of callback-style Express middleware.

---

### 2.3 Template Method (Behavioural) — View Rendering in `lib/application.js` + `lib/view.js`

Express supports EJS, Pug, Handlebars, and any other template engine without a single line of engine-specific code in its core. This is possible because `app.render()` implements the Template Method pattern: it owns the algorithm skeleton (cache check → file lookup → render), but delegates the actual rendering step to whichever engine the developer registered.

**Description:** `app.render(name, options, callback)` defines the *skeleton* of the view-rendering algorithm: look up the view in cache → resolve the file → delegate to the engine. The concrete rendering step is deferred to an interchangeable engine function stored in `app.engines`.

**Participants:**
- *Abstract Class (template)*: `app.render()` in `application.js` (lines 522–574)
- *Primitive operation (hook)*: the engine callback stored in `app.engines[ext]` — set via `app.engine(ext, fn)`
- *Concrete Class*: any compliant engine (EJS, Pug, Handlebars — each provides a `__express` export)
- *Supporting class*: `View` (`lib/view.js`) handles the file-system lookup sub-step

**Code link:** https://github.com/expressjs/express/blob/master/lib/application.js#L522  
https://github.com/expressjs/express/blob/master/lib/view.js#L133

**Problem solved:** Express must support an open-ended set of template languages without modifying its core. The Template Method pattern fixes the algorithm structure (caching, lookup, error handling) while keeping the rendering step open for extension. This satisfies the Open/Closed Principle: new engines can be plugged in without touching Express source code.

**Alternative:** A pure Strategy pattern (inject a renderer object with a `render` method) would be equally extensible and slightly more testable in isolation, since the algorithm skeleton would not be inside `app.render`. However, it would require engine authors to wrap their renderers in an object, whereas the current convention of a plain `fn(path, options, callback)` is simpler to implement.

---

### 2.4 Strategy (Behavioural) — Pluggable ETag and Query Parser in `lib/utils.js`

This is a more contained application of the Strategy pattern than the previous examples — it operates at the level of two utility functions rather than a whole subsystem — but it illustrates the principle clearly and has a direct impact on runtime performance. The key insight is that Express compiles a configuration value into a concrete algorithm function once at startup, so the hot request path never branches on settings.

**Description:** `compileETag(val)` and `compileQueryParser(val)` are strategy-selector functions. Given a setting value (string, boolean, or custom function), they return a *strategy function* — a concrete algorithm conforming to a common interface — which is then stored on the app settings and invoked at request time.

**Participants:**
- *Strategy interface*: `(body, encoding?) => string` for ETag; `(str) => object` for query parsing
- *Concrete strategies*: `exports.etag` (strong), `exports.wetag` (weak), `querystring.parse` (simple), `parseExtendedQueryString` (extended), or a user-supplied function
- *Context*: `app` object — stores the compiled strategy via `app.set('etag fn', compileETag(val))`
- *Strategy selector*: `compileETag()` / `compileQueryParser()` in `utils.js` (lines 130, 162)

**Code link:** https://github.com/expressjs/express/blob/master/lib/utils.js#L130  
https://github.com/expressjs/express/blob/master/lib/utils.js#L162

**Problem solved:** Different deployments have different performance and correctness requirements for ETag generation (weak vs strong) and query string parsing (simple vs extended/qs). Rather than branching inside every request handler, the strategy is compiled once at startup from a simple configuration value. This keeps hot-path code branch-free and allows users to inject custom implementations without patching the framework.

**Alternative:** Conditional branching inside the request handling loop (`if (settings.etag === 'weak') { ... }`) would work but adds latency on every request and couples the request path to configuration logic. A plugin registry (Map of named strategies) would be more extensible but is overkill for a binary or ternary choice.

---

## 3. Summary

Express.js is a textbook example of the **separation of concerns** principle applied at the framework level. Its dependency graph is deliberately shallow: only `response.js` (the most feature-rich module) has a wide fan-out, while utility and view modules are pure leaves.

The design patterns in use reflect the framework's two overriding goals:

1. **Extensibility without modification.** The Factory pattern hides application wiring; the Template Method pattern opens rendering to any compliant engine; the Strategy pattern makes ETag and query parsing swappable from configuration.
2. **Runtime composability.** The Chain of Responsibility (middleware stack) allows application behaviour to be assembled from independent, single-purpose functions, which is the defining feature of the Express programming model.

The main design tension is between the simplicity of callback-based middleware (Chain of Responsibility) and modern async error propagation. This is an acknowledged limitation of the Express 4/5 design that competing frameworks (Koa, Fastify, Hono) have addressed with different trade-offs.
