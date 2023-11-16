# ADR 018: Integration of IPFS Services

## Changelog
* 2023-11-16: Initial draft

## Status
DRAFT Not Implemented

## Abstract
This ADR evaluates the integration of InterPlanetary File System (IPFS) services to enhance our distributed storage capabilities, comparing various providers to determine suitability. It explores the advantages of joining an IPFS network, running a node, and the potential revenue streams.

## Context
With the increasing need for decentralized storage solutions, integrating IPFS services could be beneficial for our Next.js/Hasura GraphQL stack. IPFS is a peer-to-peer network for storing and accessing files, websites, applications, and data.

## Benchmarking
The leading IPFS service providers include:
- [Pinata](https://www.pinata.cloud/): Simplifies the process of storing and managing data on IPFS.
- [Infura](https://www.infura.io/): Provides easy-to-use API and gateway access to IPFS.
- [Fleek](https://fleek.co/): Offers hosting and storage solutions based on IPFS.

### Pros
- **Decentralization**: Enhances data redundancy and resistance to censorship.
- **Efficiency**: Content-addressable storage ensures quicker data retrieval.
- **Community Support**: Active development and community engagement.

### Cons
- **Complexity**: Initial setup and understanding IPFS may be challenging.
- **Costs**: Running a node incurs hardware and bandwidth expenses.
- **Volatility**: As a relatively new technology, IPFS lacks the maturity of traditional systems.

## Changes
To join an IPFS network:
1. Install IPFS on a local machine or cloud server.
2. Initialize the IPFS node.
3. Connect to the IPFS network and start adding content.

Advantages of running a node:
- **Data Sovereignty**: Full control over the data hosted.
- **Network Support**: Contribute to the robustness and resilience of the network.

Costs:
- Hardware acquisition for running the node.
- Bandwidth costs associated with data transfer.

Potential revenue streams:
- **Hosting Services**: Offer IPFS-based hosting for a fee.
- **Pin Services**: Charge for pinning others' content to ensure availability.
- **Content Distribution**: Monetize the distribution of popular content.

## TDD REQUIREMENTS
- Develop tests for connectivity and data retrieval within the IPFS network.
- Ensure tests cover the scalability and redundancy of the stored data.

## Cloudflare's IPFS Gateway Services

Cloudflare offers an IPFS gateway service, which acts as a bridge between the IPFS network and the traditional web. This service allows users to access IPFS content through Cloudflare without needing to run an IPFS node themselves.

### Comparison with Other Services
Unlike Pinata, Infura, and Fleek, which provide pinning and infrastructure services, Cloudflare's gateway focuses on improving access speed and reliability of IPFS content. Cloudflare leverages its global network to cache content closer to the end-user, reducing latency.

### Pros
- **Performance**: Cloudflare's extensive CDN network accelerates content delivery.
- **Ease of Access**: Simplifies the process of accessing IPFS content via HTTP(S).
- **DDoS Protection**: Cloudflare's infrastructure offers protection against DDoS attacks.
- **SSL/TLS Encryption**: Automatic HTTPS for IPFS content accessed via Cloudflare.

### Cons
- **Centralization Risk**: Reliance on Cloudflare's gateway can introduce a point of centralization.
- **Limited Control**: Less control over the content distribution compared to running a personal IPFS node.
- **No Pinning Services**: Cloudflare does not offer persistent storage solutions (pinning) like other providers.


## References
- IPFS Documentation for technical specifications and setup guidelines.
- Comparative analysis of service providers for cost-benefit insights.
