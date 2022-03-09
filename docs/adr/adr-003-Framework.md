# ADR 003: Framework

## Changelog
* 2022-03-04: Initial draft

## Status
DRAFT Not Implemented

## Abstract
Modern JS Frameworks allow for app-like in-browser experiences when deployed as PWAs (Progressive Web Apps).

## Context

### Background & Motivation
Traditional hosting solutions provide vertical or horizontal scaling, requiring the increase of resources of a particular server, or the replication of server instances (MAM)

Instead, modern hosting frameworks use static assets deployed to all edge servers and allow unlimited resources under sudden peak loads. This approach is referred to as a [JAMStack](https://jamstack.org/why-jamstack/) (Javascript, APIs & Markup ) and provides better security, scaling, performance, maintainability, portability, and devX. 

### Goals
* Use CDN Edge Hosted Static Assets
* Do not use traditionally hosted single server (i.e. Heroku, Digital Ocean, Linode)
* No mono-systems. Separate concerns as module services.

### Options Considered
* CloudFlare
* Netlify
* Stackpath
* Fastify
* Vercel

## Proposal 
Speed is the main objective. Use market leader with largest distributed network, largest feature set, and lowest costs.

### Timeline
72 Days

## Decision
Cloudflare has largest network, highest number edge location servers in over 250 cities globally to serve low-latency content to clients in all regions.

CloudFlare Pages uses a github based workflow that provides JS frameworks built at the edge with npm/yarn/webpack.

[CloudFlare Workers](https://developers.cloudflare.com/workers/) allow Serverless Functions at the edge with NPM package support.

CloudFlare KV Store allows for database connections at the edge for fast storing and lookups with eventual consistency. Durable Objects have also emerged to provide a truly serverless approach to storage and state: consistent, low-latency, distributed, yet effortless to maintain and scale. They also provide an easy way to coordinate between clients, whether it be users in a particular chat room, editors of a particular document, or IoT devices in a particular smart home. [Intro to DurableObjects](https://blog.cloudflare.com/introducing-workers-durable-objects/)

Interesting Notes: CloudFlare is continuing to develop [Mutual TLS with gRPC](https://developers.cloudflare.com/api-shield/products/mtls/) support and [IPFS services](https://developers.cloudflare.com/distributed-web/ipfs-gateway/), which may prove useful for serving decentralized applications and connecting IoT devices.

## Consequences
* Most of front end assets remain unchained. 
* Worker functions provide a modular folder structure which would be operable as well with a single Nodejs server framework. Only root of API endpoint might need to be changed.
* Dynamic php rendering of front-end content will become obsolete
* PHP/MySQL queries may still provide an intermittent API to font-end without having to change database structure nor query logic. 

### Backwards Compatibility
* New site could be deployed on a traditional, single server node. However, host would need to support a NodeJS runtime.
* New site will not be compatible with traditional Apache/Nginx with PHP hosting.

### Positive
Modern. Fast. Static. Static sites load fast, can be deployed globally. Dynamic content progressively loaded via API and Serverless Functions, infinite scalability. Practically zero server maintenance nor server load considerations.

### Negative
Up-front build times. Intricate balancing between application view frame and dynamic content. 

### Neutral
N/A

## Further Discussions
Advantages of [JAMStack](https://jamstack.org/).

## Test Cases
CI/CD framework will provide for staging location for automated e2e testing prior to pushing to live site.

## References
* https://developers.cloudflare.com/
* https://www.cloudflare.com/network/
* https://www.fastly.com/network-map/
* https://blog.intricately.com/cdn-industry-trends-market-share-customer-size#:~:text=The%20top%20CDN%20providers%20by,to%20over%201%20million%20customers





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

