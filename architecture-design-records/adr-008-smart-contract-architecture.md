# ADR 008: Smart Contract Architecture

## Changelog
* 2023-05-02: Initial draft
* 2023-11-16: Updated

## Status
DRAFT Not Implemented

## Abstract
This ADR assesses the suitability of smart contract platforms for our project, focusing on EVM (Ethereum Virtual Machine) compatibility as a primary criterion. We consider Ripple (XRP) with its upcoming EVM sidechain, Polygon, and Cosmos IBC (Inter-Blockchain Communication) with CosmWasm as potential platforms.

## Context
Our project requires a robust smart contract platform that is versatile, widely adopted, and supports complex smart contracts. The platform must also have a significant market presence and ecosystem support.

## Benchmarking
The EVM is the most widely adopted smart contract execution environment, with numerous blockchains adopting EVM compatibility to leverage the existing tools and developer community of Ethereum.

EVM-Compatible Chains:
- Ethereum Mainnet
- Binance Smart Chain (BSC)
- Avalanche C-Chain
- Fantom Opera
- Polygon (Previously Matic Network)
- Huobi ECO Chain (HECO)
- Harmony
- xDAI Chain
- Celo
- Moonbeam (Polkadot EVM)
- And more, with new chains emerging regularly.

Considering Ripple (XRP), it has a high market cap and a solid foundation that could potentially support our project. However, its current smart contract capabilities, through its Hooks system, are limited compared to EVM's capabilities. The upcoming EVM sidechain could open up more possibilities but is still under development.

Cosmos IBC operates differently, using CosmWasm for smart contracts, which allows writing smart contracts in Rust. As of now, there are discussions and plans for EVM compatibility within the Cosmos ecosystem, but these are not as mature as the established EVM chains.

### Pros
- **EVM**: Extensive developer tools, existing smart contracts, and a large developer community.
- **XRP EVM Sidechain**: Potential for high throughput and low fees, backed by a large market cap and foundation.
- **Cosmos IBC/CosmWasm**: Offers interoperability across different blockchains with a focus on customizability.

### Cons
- **EVM**: Potential scalability issues and higher transaction fees on the main Ethereum network.
- **XRP EVM Sidechain**: Still in development, unproven in real-world scenarios.
- **Cosmos IBC/CosmWasm**: Less mature in terms of developer tools and community size compared to EVM.

## Conclusion
The EVM ecosystem offers the best option due to its maturity, wide adoption, and extensive list of compatible chains. Ripple’s upcoming EVM sidechain and the current Hooks system present interesting opportunities, but they lack the maturity and ecosystem of EVM. Cosmos IBC and CosmWasm are promising for interchain operability but do not currently match the EVM's level of ecosystem development and solidity support.

## TDD REQUIREMENTS
- Test for cross-chain compatibility and smart contract migration between EVM-compatible chains.
- Evaluate Ripple's Hooks system and upcoming EVM sidechain for performance and feature set.
- Monitor developments in the Cosmos ecosystem for EVM compatibility initiatives.

## References
- EVM compatibility lists from Ethereum development resources.
- Ripple development documentation for Hooks and EVM sidechain updates.
- Cosmos IBC and CosmWasm official documentation and community proposals for EVM integration.
