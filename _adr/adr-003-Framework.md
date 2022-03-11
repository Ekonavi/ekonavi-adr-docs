# ADR 003: Framework

## Changelog
* 2022-03-05: Initial draft

## Status
DRAFT Not Implemented

## Abstract
Modern JS Frameworks allow for app-like in-browser experiences when deployed as PWAs (Progressive Web Apps). These frameworks, when built with modular, JAMStack principles implement solutions for interactive UX, SEO content management, and mobile plus desktop distribution using selective JS scripting and responsive CSS.

## Context

### Background & Motivation
[Over 50%](https://www.statista.com/statistics/277125/share-of-website-traffic-coming-from-mobile-devices/) of visitors access websites via mobile devices, and the trend continues to grow. However, most focused work and interaction with a platform still takes place on desktop devices. In order to reach both of these markets most quickly, first platform iteration will use PWA strategy with a mobile-first responsive CSS approach.

### Goals
* Manage one code base for Mobile + Desktop users
* Mobile-first design
* Progressively add complexity and features via encapsulated, modular component library

### Options Considered™
* NextJS
* Gatsby
* NuxtJS
* Angular Universal

## Proposal 
The JS Framework chosen should prioritize, speed, developer experience, developer support, developer familiarity, ecosystem of components & services, and tooling.

### Timeline
July 1st, 2022 Launch Date

## Decision
NextJS is the [most widely used open-source JS framework](https://www.npmtrends.com/angular-universal-express-vs-gatsby-vs-next-vs-nuxt) for static site generation. NextJS is [based on ReactJS](https://www.npmtrends.com/@angular/core-vs-react-vs-vue) which is the most widely used open-source JS framework and has the most mature ecosystem. For long-term maintainability and interoperability, it is almost certain that there will always be React developers available to maintain and contribute to project.

NextJS 9.3 has new SSG features (getStaticPaths,getStaticProps) that allow the combination of SSR with SSG. 

## Consequences


### Backwards Compatibility


### Positive
React most widely used Javascript framework. Codebase has potential to port to React Native for a native mobile app.

### Negative
Framework structures are specific. If React-based framework is chosen, it would need to be rewritten to change to an alternative.

### Neutral
N/A

## Further Discussions
Advantages of [JAMStack](https://jamstack.org/).

## Test Cases
CI/CD framework will provide for staging location for automated e2e testing prior to pushing to live site.

## References