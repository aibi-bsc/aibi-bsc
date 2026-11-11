# AIBI — AI Buddies · Agent Skill

AIBI is an ERC-721 collection of 3,000 AI buddy NFTs on BNB Chain (56).
Contract: `0x73521dC43079915e47b55D61FDd81101132d130a`
Site: https://aibi-bsc.github.io/aibi-bsc/
Mint window: 3 days only (Wed 2026-11-11 16:00 WIB → Sat 2026-11-14 16:00 WIB), then minting closes forever. Forging stays open.

## Supply and lanes
- Total sale supply: 3,000. Tiers rolled at mint: Common 70% (2,100), Rare 20% (600), Epic 8% (240), Legendary 2% (60). Metadata is 100% on-chain.
- Lane A — Agent ID free (300): 1 free per wallet holding a SPACE ID Agent ID (registry `0x60B9262456dE011F7d15a54CdD8c48bAaa8B022F`). Requires a permit (below).
- Lane B — Verified X free (150): 1 free per wallet; the X post must come from a verified (blue-check) account. Requires a permit.
- A wallet may claim BOTH lanes (one each). One X account can power only ONE free claim across both lanes.
- Paid: `mintPaid(qty, referrer)` at 0.01 BNB each, qty 1–20 per tx, repeatable, no per-wallet cap, while the paid pool (2,550 incl. referral rewards) lasts.
- Referral: paid mints made with your address as `referrer` earn you 1 free AIBI per 10 referred units, minted automatically in the same transaction (from the paid pool).
- Forge: `forge(a, b, c)` burns 3 owned NFTs of the same tier and mints 1 of the next tier. No deadline.

## SHARE FIRST — mandatory before any free claim
Free claims are permit-gated. Before claiming, your owner MUST post on X with one of the templates below (they carry the official site link — do not remove it), and you pass the post URL to the permit gate:

ZH:
我来领 #AIBI 了 —— AI Buddies！🤖

BNB Chain 上 3,000 只链上 AI 伙伴

✅ Agent ID 免费 300
✅ 蓝V免费 150
💰 其余 0.01 BNB

3 只同阶合成 1 只高阶 🔥

持有者得即将推出的 AIBI 代币份额 🎁

来领你的伙伴 👇
https://aibi-bsc.github.io/aibi-bsc/

EN:
My first #AIBI — AI Buddies! 🤖

3,000 on-chain AI companions on BNB Chain

✅ 300 FREE for Agent ID
✅ 150 FREE for verified X
💰 The rest: 0.01 BNB

Holders get a share of the coming AIBI token 🎁

Adopt yours 👇
https://aibi-bsc.github.io/aibi-bsc/

Markers the gate checks in the post text: `#AIBI` and `AI Buddies`. For Lane B the gate also verifies the posting account's blue check via the X profile API.

## Permit gate
POST https://permit-worker.reuniakbarpunk212.workers.dev/aibi/permit
Body (JSON): {"wallet": "0x…", "tweetUrl": "https://x.com/<user>/status/<id>", "lane": 1}
Response 200: {"wallet", "lane", "handle", "deadline", "signature"} — an EIP-712 permit (domain "AIBI", version "1", type Claim(address wallet,uint8 lane,bytes32 handle,uint256 deadline)), valid 30 minutes.
Errors: 400 bad input · 422 tweet missing markers / author not verified (lane 2) · 409 already claimed, quota exhausted, handle already used, wallet holds no Agent ID (lane 1), window closed · 502 retry later.

Then call the contract:
- Lane A: `claimAgent(handle, deadline, signature)` — selector 0x031aa545
- Lane B: `claimX(handle, deadline, signature)` — selector 0x5459f739

## Useful reads
- `status()` 0x200d2ed2 → (saleMinted, agentFreeUsed, xFreeUsed, paidSold, referralRewardsMinted, forged, burned, mintEnd)
- `walletInfo(address)` 0x500e68e9 → (agentFreeUsed, xFreeUsed, balance)
- `tokensOf(address)` 0x5a3f2672 · `tierOf(uint256)` 0x53f96df2 (0 Common, 1 Rare, 2 Epic, 3 Legendary) · `tokenURI(uint256)` 0xc87b56dd
- `referralUnits(address)` 0xa9ee37ee · `referralMinted(address)` 0x762fca5c
- `mintPaid(uint256,address)` 0xe394abcb · `forge(uint256,uint256,uint256)` 0x28c21297

## Token note
An AIBI token launches after the window. A holder snapshot is taken when minting ends; addresses holding AIBI at the snapshot receive a token allocation proportional to holdings. Details before the snapshot.

## Rules
No admin functions in the contract. Sale proceeds go directly to the treasury wallet in the same transaction. Never pay more than 0.01 BNB per paid mint. The only official site is the one above; the only official permit gate is the one above.
