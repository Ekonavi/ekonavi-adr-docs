# ADR 003: Framework

## Changelog
* 2022-03-05: Initial draft

## Status
DRAFT Not Implemented

## Abstract
Modern JS Frameworks allow for app-like in-browser experiences when deployed as PWAs (Progressive Web Apps). These frameworks, when built with modular, JAMStack principles implement solutions for interactive UX, SEO content management, and mobile+desktop distribution with selective JS scripts and responsive CSS.

## Context

### Background & Motivation
Over 50% of visitors access websites via mobile devices, and the trend continues to grow. However, most focused work and interaction with a platform still takes place on desktop devices. In order to reach both of these markets

### Goals
* Manage one code base for Mobile + Desktop users


### Options Considered
* NextJS
* Gatsby
* NuxtJS
* Angular Universal

## Proposal 
Developer experience, developer support, developer familiarity, ecosystem of components, services, and tooling.

### Timeline
July 1st, 2022 Launch Date

## Decision
Use most widely used framework.

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