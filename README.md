# Quick Intel OpenClaw Skills

OpenClaw skills for Quick Intel's crypto APIs with x402 v2 payment protocol.

## Skills

### 🔍 quickintel-scan

**Token security scanning** — Check any token for honeypots, scams, and risks.

- **Cost:** $0.03 USDC per scan
- **Chains:** 63 chains (Base, Ethereum, Solana, Sui, Tron, etc.)
- **Payment:** x402 v2 — pay on Base, Ethereum, Arbitrum, Optimism, Polygon, Avalanche, Unichain, Linea, MegaETH, or **Solana**
- **Idempotency:** Supported via `payment-identifier` extension
- **Use when:** "Is this token safe?", "Check for honeypot", "Audit contract"

```
quickintel-scan/
└── SKILL.md
```

### 🤖 tator-trade

**AI-powered trading** — Execute trades using natural language, receive unsigned transactions.

- **Cost:** $0.20 USDC per request
- **Chains:** 24 chains
- **Payment:** x402 v2 — pay on any supported EVM chain or **Solana**
- **Idempotency:** Supported via `payment-identifier` extension
- **Free endpoints:** Health check, async job polling
- **Operations:**
  - Trading: Buy, Sell, Swap
  - Transfers: Send, Wrap/Unwrap ETH
  - Bridging: Relay, LiFi, GasZip, deBridge
  - Perps: Avantis (Base)
  - Prediction Markets: Myriad
  - Token Launching: Clanker (Base), Pump.fun (Solana)
  - Name Registration: .base, .mega, .somi
  - Yield: Aave, Morpho, Compound, Yearn

```
tator-trade/
└── SKILL.md
```

## Installation

### ClawHub (Recommended)

```bash
clawhub install quickintel/quickintel-scan
clawhub install quickintel/tator-trader
```

### Manual

Copy the skill folders to your OpenClaw skills directory:

```bash
# User skills (shared across agents)
cp -r quickintel-scan ~/.openclaw/skills/
cp -r tator-trade ~/.openclaw/skills/

# Or workspace skills (per agent)
cp -r quickintel-scan ./skills/
cp -r tator-trade ./skills/
```

## Requirements

**No API keys needed** — these skills use x402 payment protocol. No subscriptions, no accounts.

**Compatible wallets:**
- `@x402/fetch` + `@x402/evm` (recommended — handles payment automatically)
- Local EVM wallet with viem or ethers.js (manual signing — see SKILL.md examples)
- Solana wallet (keypair via `@x402/svm`)
- AgentWallet (frames.ag)
- Vincent Wallet (heyvincent.ai)
- Sponge Wallet (paysponge.com — one-liner via `x402_fetch` endpoint)
- Lobster.cash (Crossmint-powered agent wallets)
- Any EIP-3009 compatible wallet (EVM)
- Any wallet supporting SPL TransferChecked (Solana)

**You need:**
- USDC on a supported payment chain
  - **EVM:** Base (recommended, lowest fees), Ethereum, Arbitrum, Optimism, Polygon, Avalanche, Unichain, Linea, or MegaETH
  - **Solana:** USDC on Solana mainnet
  - $0.03 minimum for Quick Intel scans
  - $0.20 minimum for Tator requests

## How x402 v2 Works

x402 is an HTTP payment protocol. No API keys, no subscriptions.

### EVM Flow

```
1. Call endpoint
2. Get 402 "Payment Required" response (PAYMENT-REQUIRED header)
3. Sign EIP-3009 payment authorization (transferWithAuthorization)
4. Retry with PAYMENT-SIGNATURE header
5. Get response (PAYMENT-RESPONSE header with settlement receipt)
```

### Solana (SVM) Flow

```
1. Call endpoint
2. Get 402 "Payment Required" response (includes feePayer in extra field)
3. Build SPL TransferChecked transaction with gateway's feePayer
4. Partially sign with your wallet
5. Retry with PAYMENT-SIGNATURE header (payload: { transaction: "<base64>" })
6. Gateway co-signs, submits, confirms on Solana
7. Get response (PAYMENT-RESPONSE header with tx signature)
```

### Multi-Chain Payment

Pay on whichever chain you have USDC. The 402 response lists all accepted networks:

**EVM:** Base, Ethereum, Arbitrum, Optimism, Polygon, Avalanche, Unichain, Linea, MegaETH.

**SVM:** Solana mainnet.

### ⚠️ PAYMENT-SIGNATURE Structure (Read This First)

If you're building the payment header manually (not using `@x402/fetch`), the payload structure is the #1 source of errors. Here's the exact decoded JSON:

```json
{
  "x402Version": 2,
  "scheme": "exact",
  "network": "eip155:8453",
  "payload": {
    "signature": "0x...",
    "authorization": {
      "from": "0xYourWallet",
      "to": "0xPayToFromResponse",
      "value": "30000",
      "validAfter": "0",
      "validBefore": "1771454085",
      "nonce": "0x...bytes32"
    }
  }
}
```

**Critical rules:**
- `signature` is a **sibling** of `authorization` inside `payload` — NOT nested inside `authorization`
- `value`, `validAfter`, `validBefore` must be **decimal strings** (e.g., `"30000"`) — not hex, not BigInt
- `x402Version: 2` must be present at the top level
- `nonce` is `0x`-prefixed bytes32 hex

See the individual SKILL.md files for complete viem and ethers.js manual signing examples.

### Idempotency (payment-identifier)

Both skills support the `payment-identifier` extension. Include a unique payment ID in your request to prevent double-charging on retries — especially important for Tator at $0.20/request.

### Discovery

Query `GET https://x402.quickintel.io/accepted` for all routes, pricing, supported networks, and input/output schemas.

## Example: Pay with `@x402/fetch` (Simplest)

```javascript
import { x402Fetch } from '@x402/fetch';
import { createWallet } from '@x402/evm';

const wallet = createWallet(process.env.PRIVATE_KEY);

// Scan a token — pays $0.03 USDC on Base
const scanResponse = await x402Fetch('https://x402.quickintel.io/v1/scan/full', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    chain: 'base',
    tokenAddress: '0xa4a2e2ca3fbfe21aed83471d28b6f65a233c6e00'
  }),
  wallet,
  preferredNetwork: 'eip155:8453'
});

const scan = await scanResponse.json();
```

## Example: Pay with Solana

```javascript
import { createSvmClient } from '@x402/svm/client';
import { toClientSvmSigner } from '@x402/svm';
import { wrapFetchWithPayment } from '@x402/fetch';
import { createKeyPairSignerFromBytes } from '@solana/kit';
import { base58 } from '@scure/base';

// Create Solana signer and x402 client
const keypair = await createKeyPairSignerFromBytes(
  base58.decode(process.env.SOLANA_PRIVATE_KEY)
);
const signer = toClientSvmSigner(keypair);
const client = createSvmClient({ signer });
const paidFetch = wrapFetchWithPayment(fetch, client);

// Scan a token — pays $0.03 USDC on Solana
const scanResponse = await paidFetch('https://x402.quickintel.io/v1/scan/full', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    chain: 'base',
    tokenAddress: '0xa4a2e2ca3fbfe21aed83471d28b6f65a233c6e00'
  })
});

const scan = await scanResponse.json();
```

## Example: Pay with Sponge Wallet (One-Liner)

```bash
curl -sS -X POST "https://api.wallet.paysponge.com/api/x402/fetch" \
  -H "Authorization: Bearer $SPONGE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://x402.quickintel.io/v1/scan/full",
    "method": "POST",
    "body": {
      "chain": "base",
      "tokenAddress": "0xa4a2e2ca3fbfe21aed83471d28b6f65a233c6e00"
    },
    "preferred_chain": "base"
  }'
```

No manual signing — Sponge handles the entire x402 flow. See the [sponge-wallet skill](https://wallet.paysponge.com) for setup.

## Example: Scan Before Trading

```javascript
// 1. Scan token ($0.03)
const scan = await quickIntelScan('base', tokenAddress);

if (scan.tokenDynamicDetails.is_Honeypot) {
  throw new Error('HONEYPOT - Do not trade');
}
if (scan.quickiAudit.has_Scams) {
  throw new Error('SCAM DETECTED - Do not trade');
}

// ⚠️ Liquidity check — scanner may miss non-standard pairs
if (!scan.tokenDynamicDetails.liquidity) {
  console.warn(
    'Liquidity not detected — token may use a non-standard pair. ' +
    'Verify via DEX aggregator before trading.'
  );
}

// 2. Execute trade ($0.20)
const trade = await tatorTrade(
  `Buy 0.1 ETH worth of ${tokenAddress} on base`,
  walletAddress
);

// 3. Sign and broadcast
for (const tx of trade.transactions) {
  await signer.sendTransaction(tx);
}
```

## ⚠️ Liquidity Note

The Quick Intel scanner checks major DEX pairs (WETH, USDC, USDT, native tokens) but may not detect liquidity for tokens paired against non-standard assets. If `liquidity: false`, verify independently via a DEX aggregator (1inch, Jupiter, Odos) before trading. See the quickintel-scan SKILL.md for full details.

## Links

- **Quick Intel:** https://quickintel.io
- **Documentation:** https://docs.quickintel.io
- **x402 Protocol:** https://www.x402.org
- **x402 EVM Spec:** https://github.com/coinbase/x402/blob/main/specs/schemes/exact/scheme_exact_evm.md
- **Gateway Discovery:** https://x402.quickintel.io/accepted
- **Support:** https://t.me/quickintel

## License

MIT
