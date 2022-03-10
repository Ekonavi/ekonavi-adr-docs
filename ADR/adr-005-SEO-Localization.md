# ADR 005: SEO & Localization

## Changelog
* 2022-03-09: Initial draft

## Status
DRAFT Not Implemented

## Abstract

## Context

### Background & Motivation

### Goals

### Options Considered

## Proposal 

### Timeline
July 1st, 2022 Launch Date

## Decision

## Consequences

### Backwards Compatibility

### Positive

### Negative

### Neutral
N/A

## Further Discussions

## Test Cases

## References




Put the content of every language in a different subdomain. For our example, you would have en.example.com, de.example.com, and es.example.com.
Put the content of every language in a different subdirectory. This is easier to handle when updating and maintaining your site. For our example, you would have example.com/en/, example.com/de/, and example.com/es/.
A directory structure would be my preferred choice, in most cases. It is very clean, and I like how the directories add to the overall authority of the entire site. This is basically the case with subdomains, as well; but, let’s be honest: subdomains are more “separate” than a directory structure as far as content segmentation is concerned.

Managing multi-regional and multilingual sites

Google recommends using different URLs for each language version of a page rather than using cookies or browser settings to adjust the content language on the page.

URL structure options
Country-specific domain
example.de

Pros:

Clear geotargeting
Server location irrelevant
Easy separation of sites
Cons:

Expensive (can have limited availability)
Requires more infrastructure
Strict ccTLD requirements (sometimes)
Subdomains with gTLD
de.example.com

Pros:

Easy to set up
Can use Search Console geotargeting
Allows different server locations
Easy separation of sites
Cons:

Users might not recognize geotargeting from the URL alone (is "de" the language or country?)
Subdirectories with gTLD
example.com/de/

Pros:

Easy to set up
Can use Search Console geotargeting
Low maintenance (same host)
Cons:

Users might not recognize geotargeting from the URL alone
Single server location
Separation of sites harder


New! Onboard your organization to Sourcegraph Cloud. Join the waitlist today to gain access to the beta!
Sourcegraph logo
Code Search
Monitoring
Batch Changes
Extensions
About Sourcegraph
Docs
repo:^github\.com/cosmos/cosmos-sdk$ file:^docs/architecture/adr-template\.md
@
/
docs /
architecture /
adr-template.md

Files

Symbols




Each site is separate content, independent, only platform shared. May even be themed differently by region.

Multilingual SEO is the act of optimizing the content on your website for different languages, so you become searchable in new markets, and people in different countries can find your website through organic search.


1. Chinese
2. Spanish
3. English
4. Hindi
5. Bengali
6. Portuguese