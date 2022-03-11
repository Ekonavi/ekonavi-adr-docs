# ADR 005: SEO & Localization

## Changelog
* 2022-03-09: Initial draft

## Status
DRAFT Not Implemented

## Abstract
Based on ADR 004 Domain Structure, SEO and Localization strategies can be decided upon. 

ADR 004 determines that each localized site is separate. Thus the content can be completely independent and only the platform shared. Both language and theme may vary by region.

### Goals
* Provide a solution that allows Market Relayers to localize and customize content to their target marketplaces without great difficulty.
* Create SEO structure that promotes best practices and high organic ranking

## Context
Multilingual SEO is the act of optimizing the content a site for different languages, to be searchable in each language market; thus people in different countries can find the website through organic search, expanding reach and network effect.

The languages most widely spoken in the world are:
1. Chinese
**2. Spanish**
**3. English**
4. Hindi
5. Bengali
**6. Portuguese**

Portuguese, English, then Spanish will be the primary languages of interest.

Site should be built with first target market language (Portuguese), with URL structure in language.

Users will have profiles to display their collection of claims and regenerative activities. A user should be able to post events and activities. These activities should be categorized with SEO in mind when published for public audiences.

For ease of sharing profile and a greater sense of ownership, should each user have a username at the domain root i.e. example.com/username

And from there, categorized the user content, such as example.com/username/events/ example.com/username/products/

This pattern is common in the most successful networks and may come as an expected feature. However, careful consideration and planning of url structures is needed, in order to avoid collisions with usernames.

Perhaps, a pre-written reserved list of important keywords should be created prior to launch, to prevent users from using one of these words as a username.

For example, these are some of words that would need to be reserved in each language:
* about
* help
* shop
* services
* products ...

### Options Considered
* Username at web root per profile, manage a reserved words list
* Username at a /profile root

## Decision
* Username at web root per profile, manage a reserved words list

## Consequences

### Backwards Compatibility

### Positive

### Negative

### Neutral

## Further Discussions

## Test Cases

## References