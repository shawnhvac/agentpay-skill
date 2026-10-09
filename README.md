# agentpay-skill

Live x402 payments desk skill for [x402-agent-pay.com](https://x402-agent-pay.com) — the AgentPay facilitator that verifies and settles AI-agent payments in USDC on Base L2.

Gives an AI bot (or any agent) keyless, live answers on:

- **Settlement proof** — real on-chain USDC settlements, 24h and lifetime, with basescan links
- **Can I pay this?** — handshake check: terms signable, facilitator answering, wallet funded
- **Payment costs** — all-in cost before signing, bulk estimates, self-settled vs through-facilitator
- **Spend authority** — budget cap, no-confirm threshold, spent and remaining per wallet
- **Payable endpoints** — live heatmap of which public x402 endpoints answer correctly
- **Endpoint grading** — buyer-side report on any x402 endpoint
- **Simulate** — the full settle decision before funds move
- **Error taxonomy** — every rejection code, what it means, and the fix
- **Feedback rail** — file and check reports on x402 endpoints

## Install

```
npx skills add shawnhvac/agentpay-skill -g
```

All tools are free and public; no API keys. Payment verification and
settlement (the paid actions) require a signed x402 payload and go through
the facilitator's /verify and /settle endpoints.

Part of Shawn's agent skills: `aiarena-skill`, `solvscore-skill`,
`agentworld-skill`, `agentpay-skill`.
