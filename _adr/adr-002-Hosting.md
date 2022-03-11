# ADR 002: Hosting

## Changelog
* 2022-03-04: Initial draft

## Status
DRAFT Not Implemented

## Abstract
Selecting a modern, CDN, CI/CD, Repo, Edge-worker, Edge-data driven host underpins the ability to achieve application goals. Low cost of entry plus easy scalability to support large future user-base.

### Goals
* Use CDN Edge Hosted Static Assets
* Do not use traditionally hosted single server instance (i.e. Heroku, Digital Ocean, Linode)
* No mono-systems. Separate concerns as modular micro0-services.

## Context
Traditional hosting solutions provide vertical or horizontal scaling, requiring the increase of resources of a particular server, or the replication of server instances (MAM)

Instead, modern hosting frameworks use static assets deployed to all edge servers and allow unlimited resources under sudden peak loads. This approach is referred to as a [JAMStack](https://jamstack.org/why-jamstack/) (Javascript, APIs & Markup ) and provides better security, scaling, performance, maintainability, portability, and devX. 

### Options Considered
* CloudFlare
* Netlify
* Stackpath
* Fastify
* Vercel

## Decision
* CloudFlare

Speed is the main objective. Use market leader with largest distributed network, largest feature set, and lowest costs.

Cloudflare has largest network, highest number edge location servers in over 250 cities globally to serve low-latency content to clients in all regions.

CloudFlare Pages uses a github based workflow that provides JS frameworks built at the edge with npm/yarn/webpack.

[CloudFlare Workers](https://developers.cloudflare.com/workers/) allow Serverless Functions at the edge with NPM package support.

CloudFlare KV Store allows for database connections at the edge for fast storing and lookups with eventual consistency. Durable Objects have also emerged to provide a truly serverless approach to storage and state: consistent, low-latency, distributed, yet effortless to maintain and scale. They also provide an easy way to coordinate between clients, whether it be users in a particular chat room, editors of a particular document, or IoT devices in a particular smart home. [Intro to DurableObjects](https://blog.cloudflare.com/introducing-workers-durable-objects/)

Interesting Notes: CloudFlare is continuing to develop [Mutual TLS with gRPC](https://developers.cloudflare.com/api-shield/products/mtls/) support and [IPFS services](https://developers.cloudflare.com/distributed-web/ipfs-gateway/), which may prove useful for serving decentralized applications and connecting IoT devices.

## Consequences

### Backwards Compatibility
* New site could be deployed on a traditional, single server node. However, host would need to support a NodeJS runtime.
* New site will not be compatible with traditional Apache/Nginx with PHP hosting.

### Positive
* Modern. Fast. Static. Static sites load fast, can be deployed globally. 
* Dynamic content progressively loaded via API and Serverless Functions, infinite scalability. 
* Practically zero server maintenance nor server load considerations.

### Negative
* Up-front build times. Site must be built up-front before each deployment, not dynamically rendered with each client request. Some of these effects can be mitigated with partial and targeted builds.
* Intricate balancing between application view frame and dynamic content.

### Neutral
* Vendor trust. Like any affordable web hosting solution on an internet backbone, operations rely on vendor technical uptime as well as geopolitical influences.
* However, because of the modular nature of the design and repo managed content, deploying to alternative CDN hosted options should prove to be as easy as connecting Github repo and updating DNS.
* Will need to depend on caching accuracy and freshness of content at the edges. CDN technology has been used for many years and is considered a mature technology. In addition, deployment framework provides cache-busting techniques with unique asset hashes.
* MailServer will need to be run as a separate MX service apart from domain A records. Email hosting needs to remain with current host, or moved to host that specializes in just email hosting.

## Further Discussions

## Test Cases
* CI/CD framework will provide for staging location for automated e2e testing prior to pushing to live site.
* Deploy and test performance of single page first to compared to current site, before proceeding to build-out entire system.

## References
* [https://developers.cloudflare.com/](https://developers.cloudflare.com/)
* [https://www.cloudflare.com/network/](https://www.cloudflare.com/network/)
* [https://www.fastly.com/network-map/](https://www.fastly.com/network-map/)
* [https://blog.intricately.com/cdn-industry-trends-market-share-customer-size](https://blog.intricately.com/cdn-industry-trends-market-share-customer-size)
* [https://jamstack.org/](https://jamstack.org/)