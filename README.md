# lazy-init-for-cf-workers

Utility function for lazy initialization in Cloudflare Workers Runtime.

## Problem

Cloudflare Workers don't allow async operations in the global scope. If you try to initialize resources like TanStack DB collections at module load time, you'll encounter errors.

## Usage

```ts
import { lazyInitForCFWorkers } from 'lazy-init-for-cf-workers';

// Wrap your resource initialization in a factory function
export const myCollection = lazyInitForCFWorkers(() =>
  createCollection(queryCollectionOptions({
    queryKey: ["myData"],
    queryFn: async () => fetchData(),
    // ... other options
  }))
);

// Use it normally - initialization happens on first access
const data = await myCollection.fetch();
```

## How It Works
`lazyInitForCFWorkers` wraps a factory function in a Proxy to defer initialization until first access at runtime. This prevents async operations from running in the global scope while maintaining a clean, singleton-like API.
