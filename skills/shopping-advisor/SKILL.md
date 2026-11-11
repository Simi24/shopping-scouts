---
name: shopping-advisor
description: Shopping advisor — given a need or a product link (Amazon, AliExpress, any store), checks whether the price is actually good using its history, looks for better-value alternatives on Amazon and AliExpress, reads the recurring defects in the reviews, explains what the extra money buys, and prepares the cart. Use it when the user wants to buy something, asks whether a deal is good, sends a product link, or asks what to get or where ("I need X", "is this a good deal?", "should I buy this?", "what should I get?", "where is it cheaper?").
---

# shopping-advisor

Helps a person decide **what to buy, where, and at what price**. It orchestrates three tools; each has its own skill with the details:

| Tool | Gives | Cost |
|---|---|---|
| `ali-scout` | AliExpress prices, verdict against the 30-day low, orders, reviews | free (blocks after ~12 searches in a row) |
| `amazon-scout` | Amazon prices, review summary by topic, seller, cart link | 1 SerpApi search per search/product call |
| `price-history` | price history by store from Google Shopping, deal verdict | 1 SerpApi search per call |

If a tool is missing, follow the setup section of its skill (install with `uv tool install git+https://github.com/Simi24/<tool>`, SerpApi key with `<tool> setup`). Work with whatever is available and say what is missing.

## The rules

1. **The cheapest option that covers every real need, reliably.** Below that line quality really drops (missing features, no spare parts, poor support); above it the person pays for edge cases they do not have. A flagship is worth it only when its strengths solve a problem the person actually has.
2. **The strikethrough price lies.** It is often the launch list price, or the price a product bounces back to between promotions. Judge on the **history** (`price-history`) or, on AliExpress, on the **30-day low** that EU law requires.
3. **Always answer three questions**: is the price good? are there better-value alternatives? does the person need what the extra money buys? Answering only the first is the classic mistake.
4. **Context first.** What does the person already own, what are the physical constraints (sizes, space, compatibility), how will they use it? Ask for the **one** missing fact that decides the choice before searching.
5. **Official sellers for branded products**: the brand's store, or sold by the marketplace itself. Unknown third-party sellers are a risk.
6. **Negative reviews say more than stars.** Topics with many negative mentions (Amazon) or reviews with 3 stars or fewer (AliExpress) are the recurring defects. 4.9 stars from 15 reviews is worth less than 4.6 from 2,000.
7. **AliExpress for accessories and components** (sensors, cables, gloves); less so for sized clothing or items where support and returns matter. Prices differ between AliExpress storefronts: compare when the person can buy from another one.
8. **The person always pays.** Prepare the cart (Amazon cart link, AliExpress product links); never claim to have bought anything.

## The procedure

1. **Understand the need**: read whatever the agent knows about the person (notes, memory, previous purchases); ask for the one decisive missing fact.
2. **Check the price**: current price from the store tool; for items above ~100 in local currency, `price-history product <id> --price <price seen>`. Google's current offers can lag a few days: the store's own price wins.
3. **Find alternatives**: 2-3 options — same tier, **one tier down**, and AliExpress if it is an accessory or a component. For each: price, rating and review count, recurring defects.
4. **Recommend**: one clear pick, why, what the extra money buys compared with the alternative and whether it matters to this person, the defect to watch, the link. Mention the return window when the item is expensive.
5. **Record** the decision wherever the agent keeps notes, so the next purchase starts from what was learned.

## Budget

SerpApi's free plan has 250 searches a month, possibly shared with other tools. A big decision typically costs ~5 searches (1 Amazon search, 2 product pages, 2 for the history). Check `amazon-scout credits` before a large comparison; skip `price-history` for cheap items when credits are low. Answers are cached for 24 hours.

## Answering in chat apps

When the answer goes to a messaging app, use plain text: no wide tables, one option per short line ("Qrevo S Pro · 299.99 · ★4.5 from 5,401 · all-time low"), full links on their own line.
