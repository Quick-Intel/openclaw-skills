# Supported Chains Reference

Complete reference for all 24 chains supported by Tator and 63 chains supported by Quick Intel.

## Quick Intel Scan Chains (63)

Quick Intel supports security scanning on these chains:

| Chain | Chain | Chain | Chain |
|-------|-------|-------|-------|
| eth | arbitrum | bsc | opbnb |
| base | core | linea | pulse |
| zksync | shibarium | maxx | polygon |
| scroll | polygonzkevm | fantom | avalanche |
| bitrock | loop | besc | kava |
| metis | astar | oasis | iotex |
| conflux | canto | energi | velas |
| grove | mantle | lightlink | optimism |
| klaytn | solana | radix | sui |
| injective | manta | zeta | blast |
| zora | inevm | degen | mode |
| viction | nahmii | real | xlayer |
| tron | worldchain | apechain | morph |
| ink | sonic | soneium | abstract |
| berachain | unichain | hyperevm | plasma |
| monad | megaeth | | |

**Non-EVM chains:** solana, radix, sui, injective, tron

## Tator Trading Chains (24)

These chains support all Tator operations (buy, sell, swap, send, etc.).

| Chain | Chain ID | Native Token | RPC Endpoint | Block Explorer |
|-------|----------|--------------|--------------|----------------|
| ethereum | 1 | ETH | https://eth.llamarpc.com | https://etherscan.io |
| base | 8453 | ETH | https://mainnet.base.org | https://basescan.org |
| arbitrum | 42161 | ETH | https://arb1.arbitrum.io/rpc | https://arbiscan.io |
| optimism | 10 | ETH | https://mainnet.optimism.io | https://optimistic.etherscan.io |
| polygon | 137 | MATIC | https://polygon-rpc.com | https://polygonscan.com |
| avalanche | 43114 | AVAX | https://api.avax.network/ext/bc/C/rpc | https://snowtrace.io |
| bsc | 56 | BNB | https://bsc-dataseed.binance.org | https://bscscan.com |
| linea | 59144 | ETH | https://rpc.linea.build | https://lineascan.build |
| sonic | 146 | S | https://rpc.soniclabs.com | https://sonicscan.org |
| berachain | 80094 | BERA | https://rpc.berachain.com | https://berascan.com |
| abstract | 2741 | ETH | https://api.mainnet.abs.xyz | https://abscan.org |
| unichain | 130 | ETH | https://mainnet.unichain.org | https://uniscan.xyz |
| ink | 57073 | ETH | https://rpc-gel.inkonchain.com | https://explorer.inkonchain.com |
| soneium | 1868 | ETH | https://rpc.soneium.org | https://soneium.blockscout.com |
| ronin | 2020 | RON | https://api.roninchain.com/rpc | https://app.roninchain.com |
| worldchain | 480 | ETH | https://worldchain-mainnet.g.alchemy.com | https://worldscan.org |
| sei | 1329 | SEI | https://evm-rpc.sei-apis.com | https://seitrace.com |
| hyperevm | 999 | HYPE | https://rpc.hyperliquid.xyz | https://explorer.hyperliquid.xyz |
| katana | 1380012617 | ETH | https://rpc.katana.network | https://explorer.katana.network |
| somnia | 50312 | STT | https://rpc.somnia.network | https://explorer.somnia.network |
| plasma | 1637450 | ETH | https://rpc.plasma.network | https://explorer.plasma.network |
| monad | 10143 | MON | https://rpc.monad.xyz | https://explorer.monad.xyz |
| megaeth | 18233 | ETH | https://rpc.megaeth.com | https://megascan.io |
| solana | — | SOL | https://api.mainnet-beta.solana.com | https://solscan.io |

## Chain-Specific Notes

### Base (Chain ID: 8453)
- **Default payment chain** for x402 (USDC payments)
- Supports: Clanker token launching, Basenames registration, Avantis perps
- Fast finality (~2 seconds)

### Solana
- **Non-EVM chain** — different address format (Base58)
- Supports: Pump.fun token launching, SPL token swaps
- Transaction format differs from EVM

### Arbitrum (Chain ID: 42161)
- Highest DeFi liquidity among L2s
- Full Aave, Compound, Yearn support
- 250ms block times

### MegaETH (Chain ID: 18233)
- Supports .mega name registration
- Emerging L2 with fast finality

### Somnia (Chain ID: 50312)
- Supports .somi name registration
- Gaming-focused L2

### Berachain (Chain ID: 80094)
- Uses BERA as native token
- Proof of Liquidity consensus

## USDC Addresses by Chain

For checking balances or setting up payments:

| Chain | USDC Address |
|-------|--------------|
| Base | `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913` |
| Ethereum | `0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48` |
| Arbitrum | `0xaf88d065e77c8cC2239327C5EDb3A432268e5831` |
| Optimism | `0x0b2C639c533813f4Aa9D7837CAf62653d097Ff85` |
| Polygon | `0x3c499c542cEF5E3811e1192ce70d8cC03d5c3359` |
| Avalanche | `0xB97EF9Ef8734C71904D8002F8b6Bc66Dd9c48a6E` |
| BSC | `0x8AC76a51cc950d9822D68b83fE1Ad97B32Cd580d` |

## Bridge Protocol Support

Tator uses these bridges for cross-chain transfers:

| Protocol | Supported Routes | Typical Time |
|----------|------------------|--------------|
| **Relay** | Most EVM chains | 30 seconds - 2 minutes |
| **LiFi** | Multi-protocol aggregator | 1-15 minutes |
| **GasZip** | Gas-efficient bridging | 1-5 minutes |
| **deBridge** | Deep liquidity routes | 5-30 minutes |

## Yield Protocol Availability

| Protocol | Available Chains |
|----------|-----------------|
| **Aave V3** | Ethereum, Base, Arbitrum, Optimism, Polygon, Avalanche |
| **Morpho** | Ethereum, Base |
| **Compound V3** | Ethereum, Base, Arbitrum, Polygon |
| **Yearn V3** | Ethereum |

## Perpetuals Availability

| Protocol | Chain | Assets |
|----------|-------|--------|
| **Avantis** | Base | ETH, BTC, SOL, and more |

## Prediction Markets

| Protocol | Chain | Market Types |
|----------|-------|--------------|
| **Myriad** | Multiple | Crypto, sports, politics |
