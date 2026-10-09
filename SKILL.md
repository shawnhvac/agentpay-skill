---
name: agentpay
description: Live x402 payments desk for x402-agent-pay.com, the AgentPay facilitator that settles AI-agent payments in USDC on Base L2. Use when the user asks about x402 payments, whether an agent can pay an endpoint, payment costs, spend authority on a wallet, live settlement proof, which x402 endpoints are payable, or how to verify/simulate an x402 payment. No API keys needed for the read tools.
---

# x402-agent-pay.com — live x402 payments desk

Goal: give the user a live, accurate read on the x402 payment rail —
facilitator health, payment costs, spend authority, settlement proof, and
which paid endpoints are actually payable.

Success means: the question is answered with fresh data from the public
facilitator endpoints, with a link the user can open to see it themselves.

Stop when: the question is answered with a verifiable link.

## What this is

- x402 is the AI-native payment protocol: a server names its price in an
  HTTP 402 response, the buyer pays in USDC on Base L2 by signing an
  EIP-712 authorization off-chain, and the facilitator verifies and
  settles it on-chain.
- x402-agent-pay.com is AgentPay's facilitator: it verifies and settles
  agent payments. Gas is paid by the facilitator, not the payer.
- Privacy rule: published settlements show tx hash, timestamp and amount
  only — payer and resource stay private. That is by design.

## Commands (all free, no key)

**Facilitator status + tool catalog**
```
curl -s -A "Mozilla/5.0" https://x402-agent-pay.com/facilitator/supported
```

**Live settlement proof (real on-chain USDC settlements)**
```
curl -s -A "Mozilla/5.0" https://x402-agent-pay.com/facilitator/settlement-pulse
curl -s -A "Mozilla/5.0" https://x402-agent-pay.com/facilitator/recent-settlements
```
settled_24h, lifetime counts, total USDC, last tx with a basescan.org link.

**Can this wallet pay that endpoint right now?**
```
curl -s -A "Mozilla/5.0" "https://x402-agent-pay.com/facilitator/handshake?url=<endpoint>&method=GET&wallet=0x<wallet>"
```
Terms signable, facilitator answering, settlement priced, wallet funded.

**Spend authority on a wallet (budget cap, no-confirm threshold, remaining)**
```
curl -s -A "Mozilla/5.0" "https://x402-agent-pay.com/facilitator/authority/<agent_wallet>"
```

**All-in cost of a payment before signing**
```
curl -s -A "Mozilla/5.0" "https://x402-agent-pay.com/facilitator/cost?amount=0.01"
```

**Projected cost of N purchases**
```
curl -s -A "Mozilla/5.0" "https://x402-agent-pay.com/facilitator/estimate?url=<paid endpoint>&qty=N"
```

**Which public x402 endpoints are payable right now**
```
curl -s -A "Mozilla/5.0" https://x402-agent-pay.com/facilitator/heatmap
```

**Grade any x402 endpoint from the buyer side**
```
curl -s -A "Mozilla/5.0" "https://x402-agent-pay.com/facilitator/report?url=<endpoint>"
```

**Simulate a settle before funds move (no money moves)**
```
curl -s -A "Mozilla/5.0" -X POST https://x402-agent-pay.com/facilitator/simulate \
  -H "Content-Type: application/json" -d '{"amount": 0.01}'
```
Returns settleable verdict, exact cost, refusal reasons and a state snapshot.

**Rejection codes and the error taxonomy**
```
curl -s -A "Mozilla/5.0" https://x402-agent-pay.com/facilitator/reasons
curl -s -A "Mozilla/5.0" https://x402-agent-pay.com/facilitator/errors
```

**File feedback on any x402 endpoint (free, rate-limited)**
```
curl -s -A "Mozilla/5.0" -X POST https://x402-agent-pay.com/x402-feedback/report \
  -H "Content-Type: application/json" \
  -d '{"kind":"docs","site":"<site>","endpoint":"<path>","detail":"<what is wrong>"}'
curl -s -A "Mozilla/5.0" https://x402-agent-pay.com/x402-feedback/status
```

## How to answer

- Always pull live. Settlement counts, gas economics and payable endpoints
  change constantly. Never answer from memory.
- Lead with the number, then one line of context. Link
  https://x402-agent-pay.com/facilitator/live for the live dashboard.
- When a user asks "can I pay X" or "will this payment clear", use
  handshake, cost and simulate in that order — no money ever moves in
  those calls.
- A 402 response from any endpoint is the payment gate working, not an
  error. Quote the terms it carries (price, asset, network, payTo,
  facilitator) — that is the endpoint telling the buyer the deal.
- If an endpoint fails twice, say the live feed is down and report it
  through the feedback rail.
