# `@eropple/nestjs-correlation-id` #
This package contains a single global middleware that operates on the
`X-Correlation-Id` header. If it doesn't exist on the incoming request, it
generates a random UUID and attaches it to the `X-Correlation-Id` header on the
request (`http.IncomingMessage`) and the response (`http.ServerResponse`)
objects within the NestJS request pipeline. If it already exists, the existing
value is copied to the response's headers.

Unlike other packages that do the same thing, this _doesn't_ allow you to change
the header name. That's because other packages in my `@eropple/nestjs-*`
ecosystem rely on `X-Correlation-Id` and this is intended to be used in
conjunction with them.. If you're not going to go use `@eropple/nestjs-data-sec`
or `@eropple/nestjs-auth`, you probably don't care - but almost everybody uses
`X-Correlation-Id` or `X-Request-Id`, so this still may work for you.

This is tested with Express; as it's a middleware it probably won't work with
Fastify. If somebody wants to add a Fastify version, pull requests are
gratefully accepted.

## Installation ##
`yarn install @eropple/nestjs-correlation-id`

## Usage ##
Add this as the _first_ middleware in your global middleware chain. (That way,
anything that comes after it--like, )

```ts
import { CorrelationIdMiddleware } from "@eropple/nestjs-correlation-id";

async function init() {
  const app: INestApplication = /* ... */

  app.use(CorrelationIdMiddleware());
  /* add other middleware, interceptors, etc. */

  await app.listen(3000);
}
```

By default, this uses `uuid/v4` to generate a value, but you can do whatever
you want by passing a `() => string` function to it. UUIDs tend to be the most
common option, though.
