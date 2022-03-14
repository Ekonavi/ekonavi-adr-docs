# ADR 001: Architecture Goals

## Changelog
* 2022-03-03: Initial draft

## Status
DRAFT Not Implemented

## Abstract
The architectural design of a system is tantamount to functionality, usability, costs, and flexibility over the life of the system.

### Goals
* Speed
* Agile & Modular Dev
* Open-source
* UX Clarity
* Incentivized Community Building

## Context
The architectural basics are broken down into the following sections:

#### 1. Front-end project development can begin with these decisions:
* [Hosting](adr-002-Hosting.md)
* [Framework](adr-003-Framework.md)
* [Domain Structure](adr-004-Domain-Structure.md)
* [SEO & Localization](adr-005-SEO-Localization.md)
* [Theming & Design](adr-006-Theming-Design.md)

#### 2. Back-end project development can begin after these TBD future ADRs:
* Content Management
* Component Strategy
* Authentication & Permissions
* Database Implementations
* API Structure

#### 3. Final ADRs needing development and planning pre-launch:
* Web3 Features 1.0 Design
* New Website & Marketplace Features
* Launch Strategy

#### Background & Motivation
Current [Ekonavi.com](https://ekonavi.com) site was built using PHP & MySQL and has no framework (build using a 'roll your own' method). Current [PageSpeed Insights](https://pagespeed.web.dev/) scores are as low as [15 out of 100](https://user-images.githubusercontent.com/9093152/157125334-9e32d878-56b6-4ce2-a3b1-7041b9ebdb4a.png).

To grow, the site will need to be modernized and agile. We will use this modernization opportunity to create an open platform and framework for future feature iterations and content management related to Market Relayer functionality.

#### Expanding upon goals:
* Speed
> Speed at all costs. Load times and the 'quickness' of interactions will be weighted greater than any other factor for decisions in architecture and UX. All speed tests for page load times must be 90 or greater. All interactions must be sub-one-second for a 4G device. Large percentage of users will be in rural areas; static assets, functions, and data sources at the edge should be utilized.

* Agile & Modular Dev
> * Always thinking micro-services. Independent components that each encapsulate their intended use and can plug-and-play into the system as needed. Each architectural piece should be able to be swapped out seamlessly or deployed in a different environment without needed to be refactored. 
> * The framework will define the interfacing surface for each aspect, such as hosting, edge functions, data source adaptors, css pre-processors, content management system and other tooling. 
> * Theming and localization should be managed in a separate fashion so that core framework updates can be deployed without overwriting core framework instances when using different themes or languages.
> No lock-in. The framework should be easily migratable to other hosting platforms, use other database back-ends, other web3 services, other css pre-processes, and even be rebuilt with other Javascript frameworks.

* Open-source
> Framework should be deployable via git clone and build, similar to the ease of spinning up a new WordPress site or create-react-app.
> Use open-source, npm components and frameworks.
> This framework design itself should be open-sourced to help grow impact communities.

* UX Clarity
> Simple UX with large texts and large clickable areas will be utilized. Long processes will be broken down into steps that fit well within a single mobile screen. Help and tool tips should be available on all screens.

* Incentivized Community Building
> Ease of use and user engagement are high priorities. Build upon current Ekonavi user-base, offering a better UX and new features, including incentives for increased network participation, and positive action that impacts earth state.

### Options Considered
Front-end Specifics covered in ADR 002-006

## Decision 
Research frameworks and tools to meet goals. Search and test products and designs, use what is available, build what is needed.

## Consequences

### Backwards Compatibility
N/A

### Positive
N/A

### Negative
N/A

### Neutral
N/A

## Further Discussions

## References
* [https://sourcegraph.com/github.com/cosmos/cosmos-sdk/-/blob/docs/architecture/adr-template.md](https://sourcegraph.com/github.com/cosmos/cosmos-sdk/-/blob/docs/architecture/adr-template.md)
* [https://github.com/cosmos/cosmos-sdk/blob/master/docs/architecture/README.md](https://github.com/cosmos/cosmos-sdk/blob/master/docs/architecture/README.md)
* [https://github.blog/2020-08-13-why-write-adrs/](https://github.blog/2020-08-13-why-write-adrs/)
* [https://adr.github.io/](https://adr.github.io/)