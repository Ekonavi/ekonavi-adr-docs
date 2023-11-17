# ADR 008: Smart Contract Architecture

## Changelog
* 2023-05-02: Initial draft
* 2023-11-16: Updated

## Status
DRAFT Not Implemented

## Abstract
This ADR assesses the suitability of smart contract platforms for our project, focusing on EVM (Ethereum Virtual Machine) compatibility as a primary criterion. We consider Ripple (XRP) with its upcoming EVM sidechain, Polygon, and Cosmos IBC (Inter-Blockchain Communication) with Evmos EVM as potential platforms.

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
- And more, with new chains emerging regularly such as Evmos.

Considering Ripple (XRP), it has a high market cap and a solid foundation that could potentially support our project. However, its current smart contract capabilities, through its Hooks system, are limited compared to EVM's capabilities. The upcoming EVM sidechain could open up more possibilities but is still under development.

## Ripple (XRP) Hooks and Capabilities

Ripple's Hooks feature is a relatively new addition to the XRP Ledger that allows for lightweight smart contract capabilities. Hooks are essentially small pieces of code written in WebAssembly that can be set to trigger on specific ledger events like transactions.

### Capabilities of XRP Hooks:
- **Settlement Logic**: Hooks can define conditions for automatic transaction settlement.
- **Fee Redistribution**: They can redistribute transaction fees, either by burning or redirecting to specified accounts.
- **Data Storage**: Hooks allow for minimal data storage on the ledger by leveraging the “Emit HookStateSet” opcode to store small amounts of state.

However, Hooks are designed to be simple and have limitations in terms of the complexity of smart contracts they can support. For instance, they are not suited for large-scale decentralized applications that require extensive on-chain logic and state.


### Ripple/Peersyst EVM Sidechain Overview

The Ripple/Peersyst EVM sidechain is a developmental extension of the XRP Ledger, bringing EVM compatibility to facilitate web3 applications. This proof of concept enables:

- **High Throughput**: Supports 1,000 transactions per second.
- **Quick Confirmation**: Produces a block every 5 seconds with 1 block finalization time.
- **EVM Compatibility**: Allows the deployment and interaction with smart contracts written in Solidity.
- **Consensus Algorithm**: Utilizes Proof of Authority (PoA) with CometBFT and `cosmos-sdk`.
- **Interoperability**: Features a bridge for direct connection to the XRP Ledger.

The sidechain is tailored for development purposes and is connected to the XRP Ledger Devnet, not the production Mainnet.

The sidechain will interact with the XRP main chain via a bridge that employs the XLS-38d standard​​. This bridge will be operated by community members who run witness servers, which suggests that smart contracts on the sidechain could be invoked both by interactions originating from the XRPL and directly on the sidechain itself​​.

### XRP NFTs and Data Storage:
- XRP Ledger supports NFTs through the XLS-14d standard, which allows for the tokenization of assets.
- Data storage for NFTs on XRP is limited, with the focus being on the tokenization aspect rather than on-chain metadata storage. Most metadata is stored off-chain and referenced by a unique identifier on the blockchain.

However, if the XRP EVM sidechain comes to fruition, there may no longer be a need to adhere to the XLS-14d standard specific to the XRP Ledger for NFTs. Instead, one could utilize the widely adopted ERC-1155 standard for minting NFTs directly on the XRP EVM sidechain. 

### XRP Multi-Signature:
- Multi-signature support in XRP allows multiple parties to jointly control an account and authorize transactions.
- It enhances security by requiring multiple signers to approve a transaction before it can be submitted to the ledger.

## Cosmos Current State
Cosmos IBC operates differently, using CosmWasm for smart contracts, which allows writing smart contracts in Rust. As of now, there are discussions and plans for EVM compatibility within the Cosmos ecosystem, but these are not as mature as the established EVM chains.

### Pros
- **EVM**: Extensive developer tools, existing smart contracts, and a large developer community.
- **XRP EVM Sidechain**: Potential for high throughput and low fees, backed by a large market cap and foundation.
- **Cosmos IBC/CosmWasm**: Offers interoperability across different blockchains with a focus on customizability.

### Cons
- **XRP EVM Sidechain**: Still in development, unproven in real-world scenarios.
- **Cosmos IBC/CosmWasm**: Less mature in terms of developer tools and community size compared to EVM.

## Polygon as EVM-Compatible Chain

Considering Polygon as our primary EVM-compatible chain instead of Ethereum mainnet addresses some of the concerns initially raised.

### Polygon 
- **Scalability**: Polygon offers better scalability solutions compared to Ethereum mainnet, with faster transaction times and lower fees.
- **Developer Ecosystem**: While robust, it is slightly less extensive than Ethereum’s but still significantly larger than that of Ripple's Hooks or Cosmos IBC/CosmWasm.
- **Interoperability**: Polygon's architecture is designed to interoperate seamlessly with Ethereum, which is beneficial for cross-chain functionality.

Given these factors, Polygon stands out as an EVM-compatible chain that offers the benefits of the Ethereum ecosystem without some of the drawbacks of the mainnet, particularly in terms of scalability and cost.

## Opportunistic Development with EVM Contracts

The future potential for EVM contracts to be supported across multiple blockchains, including Ripple's upcoming EVM sidechain and possibly within the Cosmos IBC Evmos ecosystem, makes developing on EVM a strategic move.

### Future Compatibility:
- **Ripple's EVM Sidechain**: Ripple's development of an EVM sidechain may allow EVM smart contracts to run within the XRP ecosystem, providing broader reach and integration.
- **Cosmos IBC**: If EVM realized, this would allow EVM contracts to be deployed on a multitude of interconnected blockchains within the Cosmos ecosystem.

Developing for EVM ensures that the smart contracts will be deployable on a wide array of platforms, maximizing market exposure and user accessibility. This cross-chain compatibility positions EVM contracts as a future-proof investment.

## Cross-Chain Awareness and State Synchronization

A smart contract on Polygon, or any EVM-compatible chain, could potentially synchronize state or be aware of the state of contracts on other chains through cross-chain communication protocols.

### Achieving Interconnection:
- **Cross-Chain Queries**: By utilizing chain-agnostic protocols or bridges, a contract can query the state of a contract on another chain.
- **State Mirroring**: Contracts on different chains could be designed to mirror each other's state, effectively reflecting updates across networks.

### Risks:
- **Complexity**: Cross-chain communication adds complexity and may require advanced understanding of different blockchain protocols.
- **Security**: Bridges and cross-chain protocols are potential points of vulnerability, increasing the attack surface.
- **Latency**: Synchronization between chains may introduce latency, impacting the responsiveness of applications.
- **Consistency**: Ensuring data consistency across chains is challenging and may lead to state divergence if not managed properly.

### Strategies for a Larger Market:
- **Unified Interfaces**: Develop contracts with standard interfaces that can interact with cross-chain protocols.
- **Decentralized Bridges**: Use or develop decentralized bridges to facilitate trust-minimized asset transfers.
- **Oracles and Relays**: Implement oracles or relays to monitor state across chains and trigger updates where necessary.

By carefully considering these aspects, we can interconnect assets across all EVM chains and possibly IBC, expanding our market reach while being cognizant of the associated risks.

## EVM on Cosmos: Status and Discussion

The latest developments in the Cosmos ecosystem regarding EVM integration are centered around Evmos, which is the Cosmos-based EVM chain. The Evmos team has announced plans to deprecate Cosmos transactions by Q3 2024, transitioning to support only Ethereum-formatted transactions.

### Summary of Developments:
- **Evmos**: The Cosmos implementation of Ethermint, designed to attract Ethereum developers to the Cosmos IBC ecosystem by streamlining the software to be more in line with Ethereum standards.
- **Deprecation of Cosmos Transactions**: Aims to eliminate the complexity caused by the coexistence of Cosmos and Ethereum transaction formats, especially concerning wallet and explorer development.

### Pros and Cons Discussed:
**Pros:**
- Simplifies development for those familiar with Ethereum.
- Aims to increase adoption by leveraging Ethereum's significant developer base.
- Enhances user experience by standardizing transactions to a familiar format.

**Cons:**
- Cosmos developers may face a learning curve adapting to EVM standards.
- Potential risk of centralization around Ethereum's transaction standards.
- Interim complexity and redundancy until Cosmos transactions are fully deprecated.

The initiative represents Cosmos's broader goal to integrate more closely with Ethereum's EVM and potentially increase its interoperability and attractiveness to a wider developer audience. The full implications of this transition are still under discussion, with careful consideration required for the balance between ease of adoption and the maintenance of a diverse and decentralized blockchain ecosystem.

## Interoperability of Evmos EVM Contracts with Regen Ledger via IBC and ICA

Evmos EVM smart contracts could potentially interact with Regen Ledger, a blockchain designed for ecological assets, by using the Inter-Blockchain Communication (IBC) and Interchain Accounts (ICA) protocols.

### Process Flow:
1. **Minting EcoCredits**: An Evmos EVM smart contract could issue new EcoCredits on Regen Ledger via IBC, with the details of the credits being defined within the contract.
2. **Purchasing EcoCredits**: Utilizing ICA, the contract could facilitate the purchase of EcoCredits by representing them within the Evmos ecosystem, handling the necessary cross-chain transactions.
3. **Retiring EcoCredits**: Similarly, the Evmos contract could retire EcoCredits on Regen Ledger through IBC when certain conditions are met, effectively removing them from circulation to ensure the ecological impact is accounted for.

### Considerations for Leveraging Regen Ledger:
- **Certification and Verification**: EcoCredits on Regen Ledger are already backed by a certified framework that guarantees their ecological value.
- **Complexity**: Establishing IBC and ICA connections requires significant technical effort and maintenance.
- **Market Recognition**: Utilizing an established ecological platform may lend more credibility and marketability to the assets.

### Pros:
- **Trust and Credibility**: Leveraging Regen Ledger's certified framework adds credibility to the ecological assets represented.
- **Ecological Impact**: Ensures that the ecological benefits are verified and contribute to global regeneration efforts.
- **Interoperability**: Demonstrates the capability of Evmos EVM contracts to interact with specialized blockchains.

### Cons:
- **Technical Effort**: Integrating with Regen Ledger through IBC/ICA is complex and may require considerable development resources.
- **Operational Overhead**: Ongoing maintenance and operational oversight will be necessary to manage the cross-chain interactions.

### Developing Own Certified EVM Smart Contracts:
Alternatively, creating our own EVM smart contracts that are recognized as certified assets representing ecological regeneration would involve:

- **Self-Certification Process**: Establishing a process for certifying and verifying the ecological impact of assets.
- **Market Adoption**: Building trust with users and stakeholders that the assets have tangible ecological value.
- **Regulatory Compliance**: Ensuring that the smart contracts meet any relevant environmental regulations and standards.

### Decision Criteria:
- **Alignment with Mission**: Whether leveraging Regen Ledger aligns with our ecological and technological objectives.
- **Cost-Benefit Analysis**: Evaluating the development and operational costs against the potential market and ecological impact.
- **Long-Term Strategy**: Considering if the collaboration enhances our long-term goals for sustainability and blockchain presence.

The choice to integrate with Regen Ledger or to develop our own certification framework within the Evmos EVM ecosystem is strategic and must be guided by both our commitment to ecological regeneration and the pragmatic assessment of the technical and market dynamics involved.

## ERC-6551: Enhanced NFT Functionality and Interaction

ERC-6551 introduces a significant enhancement to NFT functionality by providing smart contract capabilities to NFTs, effectively allowing them to function as smart contract wallets, known as Token-Bound Accounts (TBAs).

### Interaction with Smart Contracts:
- TBAs can interact with decentralized applications (DApps) and other smart contracts.
- Smart contracts can update the data or content of an ERC-6551 NFT, similar to a "filling station" or "dumping location."
- The TBA allows the NFT to act as a wallet, holding assets and engaging in transactions.

### Composability and Identity:
- NFTs can bundle related assets, such as tokens or other NFTs, into one profile.
- They have individual identities, allowing for unique interactions and behaviors within DApps.

### Provenance and Dependency:
- A complete history of the asset’s transactions and interactions is maintained, enhancing its provenance.
- NFTs can interact independently with on-chain assets or platforms, increasing their functionality.

### Challenges and Limitations:
- Integration with existing NFT projects and platforms may be a hurdle.
- Security needs to be robust to protect against vulnerabilities due to the NFT's increased interaction capabilities.
- There is a necessity for intuitive user interfaces to manage the complexity introduced by TBAs

## Conclusion
The EVM ecosystem offers the best option due to its maturity, wide adoption, and extensive list of compatible chains. Ripple’s upcoming EVM sidechain and the current Hooks system present interesting opportunities, but they lack the maturity and ecosystem of EVM. Cosmos IBC and Evmos are promising for future integrations and operability but do not currently match the EVM's level of ecosystem development and solidity support.

## References
- EVM compatibility lists from Ethereum development resources.
- Ripple development documentation for Hooks and EVM sidechain updates.
- Cosmos IBC and CosmWasm official documentation and community proposals for EVM integration.
