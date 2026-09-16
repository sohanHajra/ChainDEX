# ChainDEX

**Ethereum DEX prototype · AMM liquidity pools · React + ethers.js + Web3.js**

ChainDEX is an early Web3 engineering project I built to understand how a decentralized exchange works beyond the UI. The application connects a browser wallet to Uniswap V2-style router, factory, pair, and ERC-20 contracts; discovers available pools; quotes token swaps; manages token approvals; and submits swaps on-chain.

This repository is an **educational / portfolio project**, not a production exchange or audited smart-contract system.

## What the project does

- Connects an Ethereum wallet and reads ERC-20 balances and allowances.
- Follows the DEX contract hierarchy from **router → factory → pair contracts** to discover available pools.
- Reads token metadata from pair contracts and builds the set of tradable token pairs dynamically.
- Requests swap quotes with the router's `getAmountsOut` method.
- Handles the ERC-20 approval flow before a swap can be submitted.
- Executes single-hop `swapExactTokensForTokens` swaps with a minimum-output guard based on the quoted amount.
- Tracks pending, successful, and failed approval / swap states in the interface.
- Includes an earlier Node/Cranq experiment for creating Uniswap V2-style contract and liquidity-pool infrastructure.

## Why I built it

What interested me most was the market structure behind a DEX: liquidity is supplied by pools rather than a traditional order book, prices emerge from AMM mechanics, and execution depends on contract state, approvals, gas, and slippage.

Building ChainDEX gave me hands-on exposure to the plumbing behind those ideas instead of treating crypto markets as only prices on a screen. It was also one of my first projects connecting software engineering with market mechanics, which later grew into my work on market microstructure and systematic trading.

## Architecture

```text
Browser wallet
     │
     ▼
React interface
     │
     ├── read balances / allowances
     │
     ▼
DEX Router
     │
     ├── getAmountsOut(...)          -> swap quote
     ├── swapExactTokensForTokens(...) -> transaction
     │
     ▼
Factory
     │
     └── enumerate Pair contracts
              │
              ├── token0 / token1
              └── pool metadata
```

The UI loads the router, discovers its factory, enumerates pair contracts, and then uses those pairs to populate the exchange interface. A swap follows the normal ERC-20 flow: check allowance → request approval if needed → quote the trade → apply a minimum-output tolerance → submit the transaction.

## Technology

- **Frontend:** React, JavaScript, Tailwind CSS
- **Ethereum:** ethers.js, Web3.js, useDApp
- **DEX integration:** Uniswap V2 SDK / core / periphery interfaces
- **Contracts:** router, factory, pair, and ERC-20 ABIs
- **Tooling:** Yarn workspaces, Create React App

## Market / protocol concepts explored

- Automated market makers and liquidity pools
- ERC-20 approvals and allowances
- Router / factory / pair contract architecture
- On-chain swap quoting and execution
- Slippage / minimum-output protection
- Wallet-connected transaction state
- Pool and token discovery from contract state

## What I would improve today

This is an older learning project, and I intentionally keep the limitations visible rather than presenting it as production-ready software.

- The checked-in configuration targets the historical Goerli environment and should be migrated before attempting a new deployment.
- Swap input parsing currently assumes 18-decimal token amounts; production code should read and use each token's actual decimals.
- Routing is single-hop rather than searching multiple paths for the best execution.
- Slippage tolerance is a fixed 0.5%; a production UI should expose this as a user-controlled parameter and handle price-impact warnings.
- The project has not undergone a smart-contract security audit and should not be used with real funds.

Those are also useful lessons from revisiting the project: building something that works is different from building something safe, general, and production-ready.

## Repository structure

```text
packages/react-app/      React exchange interface and on-chain reads/writes
packages/contracts/      Contract addresses and ABIs
ChainDEX (...)/          Earlier Node/Cranq contract + liquidity-pool experiment
Images/                  Interface screenshots
```

## Screenshots

![Application Interface](Images/FINTECH.png)

![Wallet Connection](Images/FINTECH1.png)

![Exchange Interface](Images/FINTECH2.png)

![Exchange Interface with live data](Images/FINTECH3.png)

![Transaction Confirmation](Images/FINTECH4.png)

![Transaction Complete](Images/FINTECH5.png)

## Note

ChainDEX is a historical portfolio project. The code is useful as evidence of my early work with DEX architecture, smart-contract interaction, liquidity pools, and on-chain execution, but the original deployment should not be treated as a current production service.
