# ADR 004: Domain Structure

## Changelog
* 2022-03-06: Initial draft

## Status
DRAFT Not Implemented

## Abstract
A strategy for domain structure and localization is needed.

### Goals
* Best Developer Experience
* Best Content Creator/Writer Experience
* Best SEO Strategy
* Optionality and Flexibility in Localization

## Context
Google recommends using different URLs for each language version of a page rather than using cookies or browser settings to adjust the content language on the page.

### Options Considered
* Subdomain per local `es.example.com`
* Subfolder per local `example.com/es/`
* Cookie or session based localization (not recommended)

#### Option 1: Put the content of every language in a different subdomain option:
`es.example.com`
**PROS:** 
* Easy to set up
* Can use Search Console geotargeting
* Allows different server locations
* Easy separation of sites
* URL keywords greater authority per domain
**CONS:**
* Users might not recognize geotargeting from the URL alone (is "es" the language or country?)
* Sub-domains are more “separate” than a directory structure as far as content segmentation is concerned.

#### Option 2: Put the content of every language in a different subdirectory option:
`example.com/es/`
**PROS:** 
* Easy to set up
* Can use Search Console geotargeting, Low maintenance (same host)
* Sub-directories may add to the overall authority of the entire site (the root domain)
* Domain overall greater SEO authority
**CONS:**
* Users might not recognize geotargeting from the URL alone
* Single server location
* Separation of sites harder

## Decision 
* Subdomain per local `es.example.com`

Use sub-domain based localization, where both language, image, and features are separable by region. Main core of framework shares the same codebase, localization and customizations live in separate repo per site.

More strongly leaning towards this separation of concerns at the root of the system. Tying shared framework code together with a well-designed deployment strategy, rather than trying to piece-together/manage this separation of content at the presentation layer.

## Consequences

### Backwards Compatibility
Once site is launched with chosen domain structure, any future changes may incur SEO penalties. 

### Positive
* Utilizes separation of concerns
* Allows for highest flexibility of localized services
* Best SEO performance (in theory)
* Easier and more precise targeting for updates and builds

### Negative
* May need a complex deployment strategy to manage separation of core files vs custom theme, custom components, custom language files.

### Neutral
* Well-defined repo structure will need to be created and managed.
* Each site will need separate hosting.
* Multiple sites to manage, but: separation of concerns!

## Further Discussions
Querying for dynamic content? Sharing posts between locals? Auto-translate option?

Hard to really gauge what method would give best SEO performance, power of multiple language specific domains vs. one domain having multiple languages. 

## Test Cases
CI/CD framework will provide for staging location for automated e2e testing prior to deploying to live site.

## References
* [https://developers.google.com/search/docs/advanced/crawling/managing-multi-regional-sites](https://developers.google.com/search/docs/advanced/crawling/managing-multi-regional-sites)