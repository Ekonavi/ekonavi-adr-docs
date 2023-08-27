# ADR 014: Improve Real-time Feed Functionality

## Changelog
* 2023-08-27: Initial draft

## Status
DRAFT Not Implemented

## Abstract
This ADR proposes enhancements to real-time feeds functionality, including but not limited to posts, events, and products. We aim to achieve efficient data retrieval, scalability, and real-time updates through the integration of Cloudflare KV storage, and edge computing alongside our existing Hasura/GraphQL database.

## Context
The current implementation uses a simple state rerender mechanism for loading additional posts during infinite scrolling. This works well for a small dataset but is not scalable. The goal is to improve performance, reduce the bundle size, and offer real-time updates. Different strategies, such as Scroll-Into-View to Make Live, are considered.

## Patterns and Techniques
### Scroll-Into-View to Make Live
A common pattern to subscribe to posts only when they are in view. The following pseudo-code shows a TypeScript example:

```typescript
const inViewPostsUUIDs = React.useState([]);

useEffect(() => {
  // Subscribe to GraphQL with inViewPostsUUIDs in the query
}, [inViewPostsUUIDs]);
```

### KV Storage at Edge
Using Cloudflare's KV storage, which is already at the edge, to store individual posts with their `uuid` and `created_at` as the key.

```typescript
// CloudFlare Worker pseudo-code to get next 10 posts
async function getNextPosts(startingAt: string): Promise<string> {
  const posts = [];
  const list = await KV_NAMESPACE.list({prefix: startingAt, limit: 10});
  list.keys.forEach(key => {
    const postData = await KV_NAMESPACE.get(key.name);
    posts.push(JSON.parse(postData));
  });
  return JSON.stringify(posts);
}
```

## Benchmarking
We'll benchmark the speed of querying items from Hasura GraphQL vs. fetching them from the KV store. This will influence the final implementation.

## Data Storage
### Posts, Events, Products
Individual posts, events, and products will be stored in Cloudflare KV stores, each identified by a unique `uuid`. 

### Updating Items
When a user edits a post or when an interaction like a like/comment occurs, only the specific post identified by its `uuid` is updated in the KV store.

## Pros and Cons
### Pros
* Reduced bundle size and faster page loads.
* Efficient and faster data retrieval.
* Real-time updates.

### Cons
* Complexity in implementing real-time updates.
* Maintenance of KV storage and GraphQL database.

## References
* [Cloudflare KV Documentation](https://developers.cloudflare.com/workers/learning/how-kv-works)
* [Hasura Documentation](https://hasura.io/docs/latest/graphql/core/getting-started/index.html)

