# ADR 016: A/B Testing for Landing Pages

## Changelog
* 2023-09-29: Initial draft

## Status
DRAFT Not Implemented

## Abstract
This ADR proposes implementing A/B testing functionality for landing pages using Next.js middleware for routing and PostHog for tracking. We aim to achieve flexibility in testing different layouts, efficient data tracking, and seamless user experience.

## Context

### Goals
- Increase user onboarding
- Improve conversion rates

### Metrics
- Click-through rates for different CTA buttons, texts, and images
- Time spent on page
- Heatmaps for user activity

### Tech Stack
- Next.js for front-end (with middleware for routing)
- Hasura Postgres for database
- Vercel for deployment
- PostHog for analytics
- Middleware for edge-based routing and A/B test orchestration

### File Naming Strategy
- Variants of pages will be named with suffixes (e.g., `pagename-a.tsx`, `pagename-b.tsx`, etc.)
- The campaign details and specifics of what's being tested in each version will be maintained in a spreadsheet or within the A/B testing software.

### Benchmarking
- Next.js middleware enables efficient rerouting at the edge and is compatible with server-side generated content.
- Comparatively, client-side A/B testing can negatively impact web vitals and SEO rankings.

## Pros and Cons

### Pros
- Efficient data tracking using PostHog.
- Flexible layout testing due to Next.js' powerful features.
- Improved performance due to server-side generation and edge computing.
- Seamless user experience without noticeable delays.

### Cons
- Requires developer time and expertise to set up.
- Need to maintain additional tracking and documentation (either in a spreadsheet or within the A/B testing software).

## Changes
- New middleware files for handling A/B tests.
- Additional tracking in PostHog.

## TDD REQUIREMENTS
- Test that the middleware correctly routes to the different page versions.
- Test that PostHog accurately tracks the defined metrics.

## CODE EXAMPLES
```typescript
// Example Middleware Code (pages/marketing/_middleware.js)

import { NextRequest, NextResponse } from 'next/server'

const COOKIE_NAME = 'bucket-marketing'
const MARKETING_BUCKETS = ['original', 'a', 'b'] as const

const getBucket = () => MARKETING_BUCKETS[Math.floor(Math.random() * MARKETING_BUCKETS.length)]

export const middleware = (req: NextRequest) => {
  const bucket = req.cookies[COOKIE_NAME] || getBucket()
  const res = NextResponse.rewrite(`/marketing/${bucket}`)
  
  if (!req.cookies[COOKIE_NAME]) {
    res.cookie(COOKIE_NAME, bucket)
  }

  return res
}
```

```typescript
// Example Landing Page with PostHog (pages/marketing-a.tsx)

import { useEffect } from 'react'
import posthog from 'posthog-js'

export const MarketingAPage = () => {
  useEffect(() => {
    posthog.init('YOUR_POSTHOG_API_KEY')
    posthog.capture('visited_marketing_a')
  }, [])
  
  return (
    <div>
      <h1>Welcome to Marketing Page A</h1>
      <button onClick={() => posthog.capture('cta_clicked')}>Sign Up</button>
    </div>
  )
}
```

## References
- [Next.js Documentation on Middleware](https://nextjs.org/docs/middleware)
- [PostHog Documentation](https://posthog.com/docs)

