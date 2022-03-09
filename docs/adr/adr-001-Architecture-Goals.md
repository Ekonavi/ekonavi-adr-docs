# ADR 001: Architecture Goals

## Changelog
* 2022-03-03: Initial draft

## Status
DRAFT Not Implemented

## Abstract
The architectural design of a system is tantamount to functionality, usability, costs, and flexibility over the life of the system.

The architectural basics are broken down into the following sections:

* Hosting
* CI/CD
* Framework
* SEO & Localization
* Content Management
* Theming

## Context

### Background & Motivation
Current [Ekonavi.com](https://ekonavi.com) site was built using PHP & MySQL and has no framework (build using a 'roll your own' method). Current [PageSpeed Insights](https://pagespeed.web.dev/) scores are as low as [15 out of 100](https://user-images.githubusercontent.com/9093152/157125334-9e32d878-56b6-4ce2-a3b1-7041b9ebdb4a.png)

To grow, the site will need to be modernized and agile. We will use this opportunity to create an open platform and a framework for future feature iterations and content management.

### Goals
* Speed
> Speed at all costs. Load times and the 'quickness' of interactions will be weighted greater than any other factor for decisions in architecture and UX. All speed tests for page load times must be 90 or greater. All interactions must be sub-one-second for a 4G device. Large percentage of users will be in rural areas; static assets, functions, and data sources at the edge should be utilized.

* Agile & Modular Dev
> * Think micro-services. Independent components that each encapsulate their intended use and can plug-and-play into the system as needed. Each architectural piece should be able to be swapped out seamlessly or deployed in a different environment without needed to be refactored. 
> * The framework will define the interfacing surface for each aspect, such as hosting, edge functions, data source adaptors, css pre-processors, content management system and other tooling. 
> * Theming and localization should be managed in a separate fashion so that core framework updates can be deployed without overwriting frame instances using different themes or languages.
> No lock-in. The framework should be able to be easily migrated to other hosting platforms, use other database back-ends, other web3 services, other css pre-processes, and even be built with other Javascript frameworks.

* Open-source
> Framework should be deployable via git clone and build, similar to the ease of spinning up a new WordPress site or create-react-app.
> Use open-source, npm components and frameworks.
> Framework design itself should be open-source.

* UX Clarity
> Simple UX with large texts and large clickable areas will be utilized. Long processes will be broken down into steps that fit well within a single mobile screen. Help and tool tips should be available on all screens.

### Options Considered
Specifics covered in ADR 001-006

## Proposal 
Research frameworks and tools to meet goals. Search and test products and designs, use what is available, build what is needed.

### Timeline
72 Days

## Decision
Specifics covered in ADR 001-006

## Consequences
> This section describes the resulting context, after applying the decision. All consequences should be listed here, not just the "positive" ones. A particular decision may have positive, negative, and neutral consequences, but all of them affect the team and project in the future.

### Backwards Compatibility
> All ADRs that introduce backwards incompatibilities must include a section describing these incompatibilities and their severity. The ADR must explain how the author proposes to deal with these incompatibilities. ADR submissions without a sufficient backwards compatibility treatise may be rejected outright.

### Positive
N/A

### Negative
N/A

### Neutral
N/A

## Further Discussions
Specifics covered in ADR 001-006

## References
https://sourcegraph.com/github.com/cosmos/cosmos-sdk/-/blob/docs/architecture/adr-template.md

https://github.com/cosmos/cosmos-sdk/blob/master/docs/architecture/README.md