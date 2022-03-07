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

Current [Ekonavi.com](https://ekonavi.com) site was built using PHP & MySQL and has no framework (build using a 'roll your own' method). Current [PageSpeed Insights](https://pagespeed.web.dev/) scores are as low as 12 out of 100.

To grow, the site will need to be modernized and agile. We will use this opportunity to create an open platform and a framework for future feature iterations and content management.

### Goals & Non-Goals

* Speed
> Speed at all costs. Load times and the 'quickness' of interactions will be weighted greater than any other factor for decisions in architecture and UX. All speed tests for page load times must be 90 or greater. All interactions must be sub-one-second for a 4G device. Large percentage of users will be in rural areas; static assets, functions, and data sources at the edge should be utilized.

* Agile & Modular Dev
> Think micro-services and independent components that each encapsulate their intended use and can plug-and-play into the system as needed. Each architectural piece should be able to be swapped out seamlessly or deployed in a different environment without needed to be refactored. 
> The framework will define the interfacing surface for each aspect, such as hosting, edge functions, data source adaptors, css pre-processors, content management system and other tooling. 
> Theming and localization should be managed in a separate fashion so that core framework updates can be deployed without overwriting frame instances using different themes or languages.
> No lock-in. The framework should be able to be easily migrated to other hosting platforms, use other database back-ends, other web3 services, other css pre-processes, and even be built with other Javascript frameworks.

* Open-source
> Framework should be deployable via git clone and build, similar to the ease of spinning up a new WordPress site or create-react-app.
> Use open-source, npm components and frameworks.
> Framework design itself should be open-source.

* UX Clarity
> Simple UX with large texts and large clickable areas will be utilized. Long processes will be broken down into steps that fit well within a single mobile screen. Help and tool tips should be available on all screens.





### Dependencies 

### Options Considered / Abandoned Ideas

## Proposal 

> This section describes the forces at play, including technological, political, social, and project local. These forces are probably in tension, and should be called out as such. The language in this section is value-neutral. It is simply describing facts. It should clearly explain the problem and motivation that the proposal aims to resolve.
> {context body}

### Timeline

## Decision

> This section describes our response to these forces. It is stated in full sentences, with active voice. "We will ..."
> {decision body}

## Consequences

> This section describes the resulting context, after applying the decision. All consequences should be listed here, not just the "positive" ones. A particular decision may have positive, negative, and neutral consequences, but all of them affect the team and project in the future.

### Backwards Compatibility

> All ADRs that introduce backwards incompatibilities must include a section describing these incompatibilities and their severity. The ADR must explain how the author proposes to deal with these incompatibilities. ADR submissions without a sufficient backwards compatibility treatise may be rejected outright.

### Positive

{positive consequences}

### Negative

{negative consequences}

### Neutral

{neutral consequences}

## Further Discussions

While an ADR is in the DRAFT or PROPOSED stage, this section should contain a summary of issues to be solved in future iterations (usually referencing comments from a pull-request discussion).
Later, this section can optionally list ideas or improvements the author or reviewers found during the analysis of this ADR.

## Test Cases [optional]

Test cases for an implementation are mandatory for ADRs that are affecting consensus changes. Other ADRs can choose to include links to test cases if applicable.

## References

* {reference link}



https://sourcegraph.com/github.com/cosmos/cosmos-sdk/-/blob/docs/architecture/adr-template.md

https://github.com/cosmos/cosmos-sdk/blob/master/docs/architecture/README.md


no lock-in
flexible
modular