# ADR 005: SEO & Localization

## Changelog
* 2022-03-09: Initial draft

## Status
Implemented

## Abstract
Based on ADR 004 Domain Structure, SEO and Localization strategies can be decided upon. 

ADR 004 determines that each localized site is separate. Thus, the content can be completely independent and only the underlying platform is shared. Both language and theme may vary by region or as needed.

### Goals
* Provide a solution that allows Market Relayers to localize and customize content to their target marketplaces without difficulty.
* Create SEO structure that promotes best practices and high, organic ranking

## Context
Multilingual SEO is the act of optimizing the content of a site for different languages, to be searchable in each language market; thus people in different countries can find the website through organic search, expanding reach and network effect.

The languages most widely spoken in the world are:
1. Chinese
2. **Spanish**
3. **English**
4. Hindi
5. Bengali
6. **Portuguese**

Portuguese, English, then Spanish will be the primary languages of interest for Ekonavi.

Site should be built with first target market language (Portuguese), with a pre-defined URL structure in local language.

Users will have profiles to display their collection of claims and regenerative activities. A user should be able to post events and activities. These activities should be categorized with SEO in mind when published for a public facing audience.

For ease of sharing profile and a greater sense of ownership, each user should have a username at the domain root i.e. `example.com/username`

And from there, categorized the user content, such as `example.com/username/events/` `example.com/username/products/`

This pattern is common in the most successful networks and may come as an expected feature. However, careful consideration and planning of url structures is needed in order to avoid site structure collisions with usernames.

A pre-written reserved list of important keywords should be created prior to launch, to prevent users from using a reserved word as a username.

For example, these are some words that would need to be reserved in each language:
* /about
* /admin
* /explore
* /help
* /wallet
* /shop
* /market
* /services
* /products
* /agroforestry
* /crypto
* /blockchain
* /etc...

### Options Considered
* Usernames at web root per profile `example.com/username`, manage a reserved words list
* Usernames at a profile root i.e. `example.com/profile/username` or `example.com/p/username`

## Decision
* Usernames at web root per profile `example.com/username`, manage a reserved words list

## Consequences

### Backwards Compatibility
None. Once site is launched using strategy, there is no easy way to go back.

### Positive
* User sense of ownership first
* Ease of sharing user content / profile
* SEO keywords related to site content reserved and pre-planned
* SEO strategy mapped out and initiated early in project timeline

### Negative
* A word not included in reserve list could be registered by a user and no longer available for site content

### Neutral
* Need to create reserved word list

## Further Discussions

## Test Cases

## References
