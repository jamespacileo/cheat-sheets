# Jotai Cheat Sheet

## Core

### Atom
Create an atom with `atom`. It takes an initial value.

```jsx
import { atom } from 'jotai'

const countAtom = atom(0)
```

### useAtom
Use an atom in a component with `useAtom`. It returns the current state and a setter function.

```jsx
import { useAtom } from 'jotai'

const Counter = () => {
  const [count, setCount] = useAtom(countAtom)
  return (
    <div>
      <div>Count: {count}</div>
      <button onClick={() => setCount(count + 1)}>increment</button>
    </div>
  )
}
```

### Store
Create a store with `createStore`. It takes an initial state.

```jsx
import { createStore } from 'jotai'

const store = createStore({ count: 0 })
```

### Provider
Provide state to a component tree with `Provider`. It takes a `store` prop.

```jsx
import { Provider } from 'jotai'

const App = () => (
  <Provider store={store}>
    <Counter />
  </Provider>
)
```

## Utilities

### Storage
Sync state with local storage with `atomWithStorage`.

```jsx
import { atomWithStorage } from 'jotai/utils'

const countAtom = atomWithStorage('count', 0)
```

### SSR
Hydrate state on the server with `useHydrateAtoms`.

```jsx
import { useHydrateAtoms } from 'jotai/utils'

const App = () => {
  useHydrateAtoms([[countAtom, 0]])
  return <Counter />
}
```

### Async
Handle async actions with `atomWithPromise`.

```jsx
import { atomWithPromise } from 'jotai/utils'

const fetchCountAtom = atomWithPromise(fetchCount)
```

### Resettable
Create a resettable atom with `atomWithReset`.

```jsx
import { atomWithReset } from 'jotai/utils'

const countAtom = atomWithReset(0)
```

### Family
Create an atom family with `atomFamily`.

```jsx
import { atomFamily } from 'jotai/utils'

const countAtomFamily = atomFamily((id) => id)
```

## Integrations

### tRPC
Integrate with tRPC with `atomWithQuery`, `atomWithMutation`, and `atomWithSubscription`.

```jsx
import { atomWithQuery, atomWithMutation, atomWithSubscription } from '@trpc/jotai'

const countQueryAtom = atomWithQuery(() => ({
  query: 'count',
}))

const incrementMutationAtom = atomWithMutation(() => ({
  mutation: 'increment',
}))

const countSubscriptionAtom = atomWithSubscription(() => ({
  subscription: 'count',
}))
```

### Query
Integrate with React Query with `atomWithQuery`, `atomWithInfiniteQuery`, and `atomWithMutation`.

```jsx
import { atomWithQuery, atomWithInfiniteQuery, atomWithMutation } from 'jotai/query'

const countQueryAtom = atomWithQuery(() => ({
  queryKey: 'count',
  queryFn: fetchCount,
}))

const countInfiniteQueryAtom = atomWithInfiniteQuery(() => ({
  queryKey: 'count',
  queryFn: fetchCount,
}))

const incrementMutationAtom = atomWithMutation(() => ({
  mutationKey: 'increment',
  mutationFn: incrementCount,
}))
```

### URQL
Integrate with URQL with `atomWithQuery`, `atomWithMutation`, and `atomWithSubscription`.

```jsx
import { atomWithQuery, atomWithMutation, atomWithSubscription } from 'jotai/urql'

const countQueryAtom = atomWithQuery(() => ({
  query: COUNT_QUERY,
}))

const incrementMutationAtom = atomWithMutation(() => ({
  mutation: INCREMENT_MUTATION,
}))

const countSubscriptionAtom = atomWithSubscription(() => ({
  subscription: COUNT_SUBSCRIPTION,
}))
```

### Immer
Integrate with Immer with `atomWithImmer`.

```jsx
import { atomWithImmer } from 'jotai/immer'

const countAtom = atomWithImmer(0)
```

### XState
Integrate with XState with `atomWithMachine`.

```jsx
import { atomWithMachine } from 'jotai/xstate'

const countAtom = atomWithMachine(countMachine)
```

### Location
Integrate with the browser's location API with `atomWithLocation` and `atomWithHash`.

```jsx
import { atomWithLocation, atomWithHash } from 'jotai

/location'

const locationAtom = atomWithLocation()
const hashAtom = atomWithHash()
```

### Cache
Integrate with SWR with `atomWithCache`.

```jsx
import { atomWithCache } from 'jotai/cache'

const countAtom = atomWithCache('count', 0)
```

### Molecules
Integrate with Recoil with `atomWithMolecule`.

```jsx
import { atomWithMolecule } from 'jotai/molecules'

const countAtom = atomWithMolecule(countMolecule)
```

### Optics
Integrate with optics libraries with `focusAtom`.

```jsx
import { focusAtom } from 'jotai/optics'

const countAtom = focusAtom(baseAtom, (optic) => optic.prop('count'))
```

## Tools

### SWC
Integrate with SWC with `@swc-jotai/react-refresh` and `@swc-jotai/debug-label`.

```jsx
// .swcrc
{
  "jsc": {
    "parser": {
      "syntax": "ecmascript",
      "jsx": true
    },
    "transform": {
      "react": {
        "runtime": "automatic",
        "refresh": true
      }
    }
  },
  "plugins": [
    ["@swc-jotai/react-refresh"],
    ["@swc-jotai/debug-label"]
  ]
}
```

### Babel
Integrate with Babel with `plugin-react-refresh` and `plugin-debug-label`.

```jsx
// .babelrc
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ],
  "plugins": [
    "react-refresh/babel",
    "@jotai/babel-plugin"
  ]
}
```

### Devtools
Use devtools with `useAtomsDebugValue`, `useAtomDevtools`, `useAtomsDevtools`, `useAtomsSnapshot`, and `useGotoAtomsSnapshot`.

```jsx
import { useAtomsDebugValue, useAtomDevtools, useAtomsDevtools, useAtomsSnapshot, useGotoAtomsSnapshot } from 'jotai/devtools'

const Counter = () => {
  useAtomsDebugValue([countAtom])
  useAtomDevtools(countAtom, 'Count')
  useAtomsDevtools([countAtom], 'Atoms')
  const snapshot = useAtomsSnapshot()
  const gotoSnapshot = useGotoAtomsSnapshot()
  return (
    <div>
      <div>Count: {count}</div>
      <button onClick={() => setCount(count + 1)}>increment</button>
      <button onClick={() => gotoSnapshot(snapshot)}>reset</button>
    </div>
  )
}
```
