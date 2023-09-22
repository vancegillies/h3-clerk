# h3-clerk

Unofficial [Clerk](https://clerk.com/) middleware for H3.

Check [here](https://github.com/wobsoriano/nuxt-clerk-playground) for a demo with Nuxt.

## Getting Started

To use this middleware you should first create a Clerk application and retrieve a `Secret Key` and a `Publishable Key` for your application (see [here](https://clerk.com/docs/reference/node/getting-started)) to be used as environment variables `CLERK_PUBLISHABLE_KEY` & `CLERK_SECRET_KEY`.

### Installation

```bash
npm install h3-clerk
```

## Usage

```ts
import { createApp, createError, eventHandler } from 'h3'
import { clerkClient, withClerkAuth, withClerkMiddleware } from 'h3-clerk'

const app = createApp()

// For all routes
app.use(withClerkMiddleware())
app.use('/protected-endpoint', async () => {
  const { userId } = event.context.auth
  const { userId } = event.context.auth

  if (!userId)
    throw createError({ statusCode: 403 })

  const user = await clerkClient.users.getUser(userId)

  return { user }
})

// For a specific route
app.use(
  '/protected-endpoint',
  withClerkAuth(async (event) => {
    const { userId } = event.context.auth

    if (!userId)
      throw createError({ statusCode: 403 })

    const user = await clerkClient.users.getUser(userId)

    return { user }
  })
)
```

## TypeScript Shim

```ts
declare module 'h3' {
  interface H3EventContext {
    auth: import('@clerk/backend').SignedInAuthObject | import('@clerk/backend').SignedOutAuthObject
  }
}
```

## License

MIT
