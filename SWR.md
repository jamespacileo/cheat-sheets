## SWR Cheat Sheet

### Installation

```bash
npm install swr
```

### Basic Usage

```jsx
import useSWR from 'swr'

function Profile() {
  const { data, error } = useSWR('/api/user', fetch)

  if (error) return <div>failed to load</div>
  if (!data) return <div>loading...</div>
  return <div>Hello {data.name}!</div>
}
```

In this basic example, SWR will fetch data from '/api/user' using the fetch function. The data and error are returned from the useSWR hook and can be used to render different UI based on the data fetching state.

### Error Handling

```jsx
import useSWR from 'swr'

function Profile() {
  const { data, error } = useSWR('/api/user', fetch)

  if (error) return <div>Error: {error.message}</div>
  if (!data) return <div>loading...</div>
  return <div>Hello {data.name}!</div>
}
```

In this example, if there is an error during data fetching, it will be returned from the useSWR hook and can be used to render an error message.

### Revalidation

SWR automatically revalidates data when the user focuses on the page or reconnects to the internet. This can be controlled with the `revalidateOnFocus` and `revalidateOnReconnect` options.

```jsx
import useSWR from 'swr'

function Profile() {
  const { data, error } = useSWR('/api/user', fetch, { revalidateOnFocus: false })

  // ...
}
```

In this example, SWR will not revalidate data when the user focuses on the page.

### Mutation & Revalidation

SWR provides a mutate function that can be used to update the local data and trigger revalidation.

```jsx
import { mutate } from 'swr'

mutate('/api/user', newUser, false) // update the local data immediately, but don't revalidate
mutate('/api/user') // trigger a revalidation (refetch) to make sure our local data is correct
```

### Conditional Data Fetching

You can conditionally fetch data by passing a function as the `fetcher` argument.

```jsx
import useSWR from 'swr'

function Profile({ id }) {
  const { data, error } = useSWR(() => id ? `/api/user/${id}` : null, fetch)

  // ...
}
```

In this example, SWR will only fetch data if the `id` is not null.

### Pagination

SWR provides the `useSWRInfinite` hook for working with paginated data.

```jsx
import { useSWRInfinite } from 'swr'

function MyComponent() {
  const { data, error, size, setSize } = useSWRInfinite((index, previousPageData) => {
    // reached the end
    if (previousPageData && previousPageData.length === 0) return null

    // API url to fetch the page
    return `/api/data?page=${index + 1}`
  }, fetch)

  // ...
}
```

In this example, `useSWRInfinite` will fetch pages of data from '/api/data'. The `size` is the number of pages fetched, and `setSize` can be used to fetch more pages.

### Subscription (Real-time updates)

SWR doesn't directly support subscriptions or real-time updates, but you can easily integrate it with a real-time data source.

```jsx
import useSWR from 'swr'
import { subscribeToUserUpdates, unsubscribeFromUserUpdates } from './api'

function Profile({ id }) {
  const { data, mutate } = useSWR(`/api/user/${id}`, fetch)

  useEffect(() => {
    const unsubscribe = subscribeToUserUpdates(id, mutate)
    return () => unsubscribeFromUserUpdates(id, unsubscribe)
  }, [id, mutate])

  // ...
}
```

In this example, we subscribe to user updates when the component mounts, and unsubscribe when it unmounts. When an update is received, we call `mutate` to update the local data and trigger revalidation.

### Prefetching

You can prefetch data with the `mutate` function.

```jsx
import { mutate } from 'swr'

// prefetch the data
mutate('/api/user', fetch('/api/user').then(res => res.json()))
```

In this example, we prefetch the user data and store it in the cache. The next time `useSWR('/api/user')` is called, the data will be returned from the cache immediately.

### Middleware

SWR supports middleware, which allows you to add custom logic before or after certain SWR lifecycle events.

```jsx
import { useSWRMiddleware, createMiddleware } from 'swr'

const logger = createMiddleware((useSWRNext) => (key, fetcher, config) => {
  console.log('fetching', key, '...')
  return useSWRNext(key, fetcher, config)
})

function Profile() {
  const { data, error } = useSWR('/api/user', fetch, {
    middleware: [logger]
  })

  // ...
}
```

In this example, we create a middleware that logs a message every time we fetch data. The middleware is passed to the `useSWR` hook via the `middleware` option.
