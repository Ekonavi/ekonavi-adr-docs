# ADR 004: Domain Structure

## Changelog
* 2022-03-06: Initial draft

## Status
DRAFT Not Implemented

## Abstract
A strategy for domain structure and localization is needed.

## Context

### Background & Motivation

Google recommends using different URLs for each language version of a page rather than using cookies or browser settings to adjust the content language on the page.

### Goals
* Use CDN Edge Hosted Static Assets

### Options Considered
* sub-domain per local (es.example.com)
* sub-folder per local (example.com/es/)
* cookie / session based local (not recommended)

## Proposal 
Use sub-domain based localization, where both language, image, and features are separable by region. Main core of framework shares the same codebase, localization and customizations live in separate repo per site.

Put the content of every language in a different subdomain option:
**es.example.com**
* Pros: Easy to set up, Can use Search Console geotargeting, Allows different server locations, Easy separation of sites
* Cons: Users might not recognize geotargeting from the URL alone (is "es" the language or country?)

Put the content of every language in a different subdirectory option:
**example.com/es/**
* Pros: Easy to set up, Can use Search Console geotargeting, Low maintenance (same host)
* Cons: Users might not recognize geotargeting from the URL alone, Single server location, Separation of sites harder

### Timeline
July 1st, 2022 Launch Date

## Decision
Sub-domain per local (es.example.com).

## Consequences
* Well-defined repo structure will need to be created and managed.
* Each site will need hosting account.

### Backwards Compatibility
Once site is launched with chosen domain structure, any future changes may incur some SEO penalties. 

### Positive
Utilizes separation of concerns, allows for highest flexibility of localized services. Best SEO performance. Better targeted updates and builds.

### Negative
Complex deployment strategy will be needed to considered separation of core files and custom components, language files.

### Neutral
Multiple sites to manage, but, separation of concerns.

## Further Discussions
Sub-directories may add to the overall authority of the entire site (root domain).
Sub-domains are more “separate” than a directory structure as far as content segmentation is concerned.
Querying for dynamic content? Share posts between locals?

## Test Cases
CI/CD framework will provide for staging location for automated e2e testing prior to pushing to live site.

## References
* [https://developers.google.com/search/docs/advanced/crawling/managing-multi-regional-sites](https://developers.google.com/search/docs/advanced/crawling/managing-multi-regional-sites)