# SusuChain: community savings circles on Stellar

**Live demo:** https://sainath998.github.io/susuchain/ (runs on Stellar Testnet)

More than a billion people save through *rotating savings circles*: susu in West Africa, tanda in Latin America, chit funds in India, ROSCAs everywhere else. Everyone pays a fixed amount each round, and one member takes the whole pot, until every member has had a turn. These circles do real work, but they run on trust in one treasurer who holds the cash. When that treasurer leaves with the money, the members have no recourse.

SusuChain moves the circle's pot onto Stellar so that **no single person can take the money**.

## How it works
1. **Form a circle.** A pool account is created on Stellar. Every member is added as a signer with weight 1, all thresholds are set to a majority (for example 3 of 4), and the pool's **master key weight is set to 0**. From that point, nobody, including whoever set it up, can move funds alone.
2. **Contribute.** Each member sends the round's contribution to the pool as a native Stellar payment, with a memo like `Susu R1 contribution`. Each payment settles in about 5 seconds for a fraction of a cent.
3. **Release the pot.** The payout to this round's recipient is a single transaction, and Stellar only accepts it when a majority of members have signed it. The pot can't be released until everyone has paid. Recipients rotate until each member has received the pot once.
4. **Audit.** Every contribution and payout is public on the ledger, and the app links each one to Stellar Expert.

## Why Stellar
- Native **multisig with weighted thresholds** does the treasurer's job at the protocol level. No custom contract is needed, so there's no contract code to get wrong.
- Fees are tiny and settlement takes about 5 seconds, which matters when contributions are small.
- Anchors and USDC on Stellar give a direct path to stable-value contributions and cash in/out through local anchors (MoneyGram Access and others).

## Run it
It's a single static page with no build step:
```
open index.html        # or serve with any static server
```
The page uses `@stellar/stellar-sdk` and talks to Horizon testnet and Friendbot directly.

> Demo note: for a one-click demo, the browser generates and holds every member's testnet keys. In production, each member signs from their own wallet (Freighter/LOBSTR via Stellar Wallets Kit), and payout transactions are passed around as XDR to collect co-signatures.

## Roadmap
- Contributions in USDC, with cash in/out through SEP-24 anchors
- Each member signs from their own wallet, with payout XDR shared over a link or SMS
- A Soroban contract that enforces late-payment penalties and a randomized payout order
- On-chain contribution history as a portable credit record for people without a credit history
