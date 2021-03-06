# Fast Koa Router

## Installation

`npm install fast-koa-router`

## About

## Usage

```js
const routes = {
  get: {
    '/path': async function(ctx, next) {
      ctx.body = 'ok';
    },
    '/nested': {
      '/path': async function(ctx, next) {},
      '/path/:id': async function(ctx, next) {}
    }
  },
  post: {
    '/path': async function(ctx, next) {}
  },
  policy: {
    '/path': async function(ctx, next) {},
    '/nested': {
      '/path': async function(ctx, next) {},
      '/path/:id': async function(ctx, next) {}
    }
  },
  prefix: {
    '/': async function(ctx, next) {} // / matches all routes
  }
};

// or

const routes = {
  '/path': {
    get: async function(ctx, next) {
      ctx.body = 'ok';
    },
    post: async function(ctx, next) {},
    policy: async function(ctx, next) {}
  },
  '/nested': {
    '/path': { get: async function(ctx, next) {}, policy: async function(ctx, next) {} },
    '/path/:id': { get: async function(ctx, next) {}, policy: async function(ctx, next) {} }
  },
  prefix: {
    '/': async function(ctx, next) {} // / matches all routes
  }
};

// or

const routes = {
  get: {
    '/path': async function(ctx, next) {},
    '/nested/path': async function(ctx, next) {},
    '/nested/path/:id': async function(ctx, next) {}
  },
  post: {
    '/path': async function(ctx, next) {}
  },
  policy: {
    '/path': async function(ctx, next) {},
    '/nested/path': async function(ctx, next) {},
    '/nested/path/:id': async function(ctx, next) {}
  },
  prefix: {
    '/': async function(ctx, next) {} // / matches all routes
  }
};
```

Supports

- put
- post
- get
- delete
- patch

methods

```js
const Koa = require('koa');
const app = new Koa();
const { router } = require('fast-koa-router');

app.use(router(routes));
app.listen(8080);
```

## Policies

Policies are used to add authentication and authorization or any other kind of middleware. It is like `all` and is executed before the matching route.
They must call next in order for the actual route to be executed.
Policies will be executed even if a matching get, post, put, delete, patch is not found
Policies are executed before anything else.

## Prefixes
Prefixes are also used to add middleware. Unlike policies they will only be executed if a matching 
get, post, put, delete or patch is found. They are convenient to add authentication or authorization in many paths that start with a prefix eg: /api/v1

Prefixes are executed after policies and before other middleware.
Note than both in prefix and policy middleware ctx.params and ctx._matchedRoute are available.


## Fast

The path matching is pretty simple. Unlike other middlewares not all routes are checked so performance does not degrade with routes size.
However complex regex matching is not supported.
