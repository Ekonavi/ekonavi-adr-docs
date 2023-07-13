# ADR 003: Framework

## Changelog
* 2022-03-05: Initial draft

## Status
Implemented

## Abstract
Modern JS Frameworks allow for app-like in-browser experiences when deployed as PWAs (Progressive Web Apps). These frameworks, when built with modular, JAMStack principles implement solutions for interactive UX, SEO content management, and mobile plus desktop distribution using selective JS scripting and responsive CSS.

### Goals
* Manage one code base for Mobile + Desktop users
* Mobile-first design
* Progressively add complexity and features via encapsulated, modular component library

## Context
[Over 50%](https://www.statista.com/statistics/277125/share-of-website-traffic-coming-from-mobile-devices/) of visitors access websites via mobile devices, and the trend continues to grow. However, most focused work and interaction with a platform still takes place on desktop devices. 

In order to reach both of these markets most quickly, first platform iteration will use PWA strategy with a mobile-first responsive CSS approach.

### Options Considered
* NextJS
* Gatsby
* NuxtJS
* Angular Universal

## Decision
* NextJS

The JS Framework chosen should prioritize, speed, developer experience, developer support, developer familiarity, ecosystem of components & services, and tooling.

NextJS is the [most widely used open-source JS framework](https://www.npmtrends.com/angular-universal-express-vs-gatsby-vs-next-vs-nuxt) for SSR site deployment.

NextJS is [based on ReactJS](https://www.npmtrends.com/@angular/core-vs-react-vs-vue) which is the most widely used open-source JS framework and has the most mature ecosystem. 

For long-term maintainability and interoperability, it is almost certain there will always be React developers available to maintain and contribute to project.

Since NextJS 9.3 (12.1.0 current) has supported SSG features (getStaticPaths,getStaticProps) that allow the combination of SSR with SSG. Plus [SWR hook](https://swr.vercel.app/) now available for dynamic user profile content and interactive feeds.

Gatsby, even though feature-rich and more of a turn-key solution, will likely not provide the openness and flexibility needed for advanced customizations. Gatsby was designed primarily for SSG static blogs.

## Consequences

### Backwards Compatibility
N/A

### Positive
* React most widely used Javascript framework. 
* Codebase has potential to port to React Native for a native mobile app.

### Negative
* Framework structures are specific. 
* If React-based framework is chosen, it would need to be rewritten to change to an alternative.

### Neutral
N/A

## Further Discussions

## Test Cases
Basic site using framework will be deployed and tested, scaffolding each needed functionality before committing to developing entire platform.

## References
[https://medium.com/eincode/next-js-vs-gatsby-js-frameworks-all-you-need-to-know-4147f36ed915](https://medium.com/eincode/next-js-vs-gatsby-js-frameworks-all-you-need-to-know-4147f36ed915)
[https://www.solutelabs.com/blog/gatsby-js-vs-next-js-which-one-to-choose-when](https://www.solutelabs.com/blog/gatsby-js-vs-next-js-which-one-to-choose-when)
[https://www.gatsbyjs.com/features/jamstack/gatsby-vs-nextjs](https://www.gatsbyjs.com/features/jamstack/gatsby-vs-nextjs)
