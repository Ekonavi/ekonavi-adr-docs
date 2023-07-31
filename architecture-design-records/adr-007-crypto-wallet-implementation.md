# ADR 007: Crypto Wallet Implementation

### Changelog

* 2023-05-02: Initial draft

### Status

DRAFT Not Implemented

### Abstract

Enable users to log-in and execute blockchain transactions using a offline signer (Crypto Wallet) in order to benefit from blockchain technology features such as ownership, immutability, and transparency.

#### Goals

* Secure and Easy Authentication Option
* Low cost with lots of growth potential
* Non-custodial user-assets: Users control with wallet/seed phrase
* Users are educated and understand wallet best practices
* Easy to sign-in, sign transactions, and obsrvere user session and blockchain states
* Excellent overall UX/UI
* No lock-in with 3rd party provider

### Context

#### Options Considered

### Metamask SDK Vanilla ([https://metamask.io/](https://metamask.io/))

**Pros:**
- Widely used and popular among the crypto community
- Supports Ethereum and ERC20 tokens
- Simple and straight forward
- Browser extension available for seamless integration
- Compatible with hardware wallets (e.g., Ledger, Trezor)
- Supports EIP-712, making it easier for users to sign structured data

**Cons:**
- Limited to Ethereum-based blockchains (secp256k1 only)
- May not support newer or less popular networks
- Users must install a browser extension
- Users could potentially experience lock-in with the Metamask ecosystem

### Keplr Vanilla ([https://www.keplr.app/](https://www.keplr.app/))

**Pros:**
- Multi-chain support, including Cosmos, Kava, and Secret Network
- Non-custodial and open-source
- Browser extension available for seamless integration
- Easy to use and set up
- Allows for staking, governance participation, and token swaps
- Supports both secp256k1 and ed25519 key types

**Cons:**
- Less popular compared to Metamask
- Limited token support compared to Ethereum-based wallets
- Users must install a browser extension
- May not support newer or less popular networks

### Thirdweb ([https://thirdweb.com/](https://thirdweb.com/))

**Pros:**
- Offers a customizable and modular wallet solution
- Supports Ethereum, ERC20 tokens, and NFTs
- Provides tools for managing smart contract interaction
- Can be integrated into existing websites and applications

**Cons:**
- May require more developer resources for integration and customization
- Potentially less user-friendly than other options
- Limited to Ethereum-based blockchains (secp256k1 only)
- Users could potentially experience lock-in with the Thirdweb ecosystem

### WalletConnect ([https://walletconnect.com/](https://walletconnect.com/))

**Pros:**
- Allows users to connect their existing wallet to your application
- Supports multiple wallet providers and blockchains
- Open-source and non-custodial
- Enhances user experience by not requiring a browser extension
- QR code-based sign-in for mobile wallet users
- Supports both secp256k1 and ed25519 key types (depending on the connected wallet)

**Cons:**
- Relies on users having a compatible wallet
- May have limited support for less popular networks
- No wallet creation features

### Web3Auth ([https://web3auth.io/](https://web3auth.io/))

**Pros:**
- Uses [MPC](https://web3auth.io/docs/content-hub/guides/mpc) (Multi Party Computation) architecture
- Uses web2 social logins
- Supports secp256k1 & ed25519 encryption (multichain i.e. cosmos)
- Free up to 1000 users
- Offers a passwordless authentication system using blockchain technology
- Supports Ethereum and ERC20 tokens
- Provides a simple API for integration
- User experience benefits from not requiring a browser extension or wallet installation
- No lock-in with a 3rd party provider

**Cons:**
- Limited to Ethereum-based blockchains (secp256k1 only)
- May have limited support for less popular networks
- Users must have an Ethereum address to use the authentication system
- Potentially less secure than other wallet options

### Alchemy Web3 Wallet Tools ([https://www.alchemy.com/top/wallet-tools](https://www.alchemy.com/top/wallet-tools))

**Pros:**
- Supports Ethereum, ERC20 tokens, and NFTs
- Offers a suite of developer tools for wallet management and smart contract interaction
- Provides APIs for easy integration
- Allows for customization and scalability

**Cons:**
- Requires developer resources for integration and customization
- Limited to Ethereum-based blockchains (secp256k1 only)
- Users could potentially experience lock-in with the Alchemy ecosystem
- Potentially less user-friendly than other options

## Recommendation

[Web3Auth Modal](https://web3auth.io/docs/sdk/web/modal/)

Web3Auth includes the [wallet-connect-v2-adapter[(https://web3auth.io/docs/sdk/web/adapters/wallet-connect-v2), which enables support for all chains, including Polygon, and also supports social logins. This significantly broadens the utility and compatibility of Web3Auth compared to its initial evaluation.

Taking into account the wide variety of onboarding options and the importance of speed to deploy, Web3Auth emerges as a strong choice for implementation. Offering a passwordless authentication system using blockchain technology, it provides a simple API for integration and supports both Ethereum-based blockchains and other chains through the wallet-connect-v2-adapter. With the added benefit of social logins, Web3Auth caters to a broader user base and creates a seamless user experience.

In comparison, WalletConnect focuses on connecting users' existing blockchain wallets to web or mobile applications. Although it supports multiple wallet providers and blockchains, it does not inherently support social logins. While WalletConnect is a versatile solution, it may not be the most efficient option for projects prioritizing a wide range of onboarding options and rapid deployment.

In conclusion, given the need for a variety of onboarding options and quick implementation, Web3Auth is a more suitable choice. Its support for multiple chains, including Polygon, social logins, and the wallet-connect-v2-adapter for Cosmos based chains, combined with the ease of integration, make it an ideal solution for Ekonavi's current need of versatility and rapid deployment.
