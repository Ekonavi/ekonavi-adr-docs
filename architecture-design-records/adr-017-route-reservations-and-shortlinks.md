# ADR 017: Implementing Shortlink Functionality using Cloudflare Caching and Database Routes

## Changelog
* 2023-09-19: Initial draft
* 2023-10-05: Vanity URL Research 

## Status
DRAFT Not Implemented

## Abstract
This ADR proposes a strategy for implementing shortlinks at the apex level of the website, such as `/anycode`. The strategy relies on Cloudflare's caching and a new database table, `routes`, which will map short codes to full URIs.

## Context
Handling URLs at the apex level for various functionalities like user-generated content, shortlinks, etc., has various trade-offs in terms of speed, reliability, and maintainability. After considering options like using special characters in the URL and using a folder path, we settled on using short codes directly at the apex level (e.g., `/anycode`).

## Benchmarking
* Twitter: Handles user names and other functionalities at the apex level with high speed, possibly via extensive caching mechanisms.
* Cloudflare: Provides a robust caching mechanism at the edge level.

## Pros and Cons
### Pros
1. High speed and low latency due to edge caching by Cloudflare.
2. Simplified URL structure.
3. Easy management and mapping through a `routes` database table.

### Cons
1. Might require monitoring to ensure cache consistency.
2. Initial setup and maintenance overhead for the `routes` table and caching logic.

## Changes
Create a new database table `routes` with two text columns: `short` and `full`. Both columns will be unique, and `full` will be nullable.

## TDD REQUIREMENTS
* Unit test for database CRUD operations related to the `routes` table.
* Unit test for caching mechanism.

## References
* Cloudflare Caching API

## Code Examples
### Middleware.ts
```typescript
// Middleware for handling short codes
export default async function middleware(req: NextApiRequest, res: NextApiResponse) {
  const shortCode = req.url.split('/')[1];
  const fullURI = await getFullURI(shortCode);
  if (fullURI) {
    res.redirect(fullURI);
  } else {
    // Proceed to Next.js routing
    next();
  }
}
```

### Cloudflare Cache API
```typescript
// Function to populate Cloudflare cache
async function populateCache(shortCode: string, fullURI: string) {
  const cache = caches.default;
  let response = new Response(fullURI);
  await cache.put(shortCode, response);
}
```

### /make-short Endpoint
```typescript
// Worker function for creating shortlinks
async function makeShort(req: Request, res: Response) {
  const fullURI = req.body.fullURI;
  let shortCode = await generateShortCode(fullURI);
  res.json({ shortCode });
}
```

## Conclusion
The implementation strategy of using `/anycode` at the apex level with Cloudflare caching and a database table (`routes`) offers a high-speed, reliable, and maintainable solution for shortlink functionalities. The `/make-short` endpoint will serve as the backend logic for creating new shortlinks and populating them in both the database and Cloudflare cache. This design balances performance and maintainability while keeping the URL structure simple and intuitive.

## Research suggests Vanity URLs instead of codes should be used for shortlinks

### 1. Bitly’s Science of the Shortened URL Report:

Bitly, a popular URL shortening service, analyzed the click rates of its URLs and found that branded short domains increased the click-through rate (CTR) by up to 34% compared to generic bit.ly links.

### 2. Marketing Sherpa's Case Study:

In a case study, Marketing Sherpa found that descriptive URLs increased click-through rates by 15-25% over non-descriptive ones.

### 3. HubSpot Analysis:

HubSpot analyzed the effect of URL length on sharing and found that URLs under 60 characters had a higher distribution rate. However, this isn't a direct comparison between descriptive and coded URLs but indicates a preference for shorter URLs on platforms like Twitter, where character count matters.

### 4. Suspicion of Shortened URLs:

A study by the University of Greenwich indicated that users are often wary of clicking shortened URLs. 91% of the participants in the study indicated they'd be hesitant to click on a shortened URL in an email from an unknown sender. This suggests that while shortened URLs are useful, context matters. In situations where trust is paramount, descriptive URLs might be more effective.

### 5. SEO Impact:

Backlinko's analysis of one million Google search results found that URLs with keywords (descriptive URLs) had a slight SEO advantage over non-descriptive URLs. However, this advantage is relatively minor compared to other SEO factors.

## Addendum to Conclusion
System should allow for the generation and association of both unique short codes and vanity URLS. Vanity links will start with username and then post slug. If the username has not been set, the user's ref_code will substitute. 

## References
1. [Next.js Documentation](https://nextjs.org/docs)
2. [Cloudflare Cache API Documentation](https://developers.cloudflare.com/cache/how-to/cache-api)
3. [Twitter Engineering Blog](https://engineering.twitter.com/)
4. [Database Normalization Techniques](https://en.wikipedia.org/wiki/Database_normalization)
