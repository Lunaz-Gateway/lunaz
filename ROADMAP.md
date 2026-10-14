# Lunaz roadmap

This roadmap shows where Lunaz goes after the first version. It is a plan, not a promise. We change the order when we learn from merchants. Feedback is welcome.

The main theme is to make Lunaz work for people who do not use crypto. Most small shops in Indonesia do not hold ZEC, and most buyers do not either. A shop owner should be able to sell, get paid and see money in a bank account without learning what a viewing key is. The privacy stays the same.

Items are grouped as Now (in progress or next up), Next (the following months) and Later (after that). Each item says why it matters and what is still open.

## Where we are today

The first version works on Zcash mainnet:

- Shielded invoices with one address per invoice, detected in seconds and paid after one confirmation.
- Cross-chain payments in USDC, USDT, SOL, ETH and BTC through NEAR Intents.
- A developer dashboard with API keys, webhooks, invoices, fees, the accountant CSV export, and a fee of 0.5% inside each shielded payment, with a 0.75% app fee on cross-chain payments (0.5% plus 0.25% for the swap).
- Sunday Market, a demo marketplace with five shops, each its own Lunaz merchant.
- Withdraw to bank, in testing with an off-ramp partner.

## Theme 1: Lunaz for people who do not use crypto

### Now

**Prices in local currency.** Merchants set prices in IDR or USD. Lunaz converts to ZEC when the buyer opens the checkout and locks the rate for the life of the invoice. The dashboard, receipts and the CSV show the local currency first and ZEC second.
Open: which rate source to trust, and how long a rate lock can last before the merchant carries too much price risk.

**Withdraw to bank with TransFi.** Let a merchant turn ZEC into IDR in a BCA, Mandiri, BRI or BNI account through TransFi, a licensed off-ramp partner.
Open: whether TransFi accepts ZEC directly or needs USDC first, and whether the deposit address is transparent.

**Plain-language mode.** A setting that hides crypto terms in the dashboard. "Viewing key" becomes "read-only wallet key", "shielded address" becomes "private payment address", and amounts show in IDR. The technical terms stay one click away for developers.

### Next

**Pay with QRIS, bank transfer or card.** Buyers who do not hold crypto pay with the methods they already use, through an on-ramp partner. The partner buys ZEC and sends it shielded to the invoice address. For the merchant, it is a normal Lunaz payment.
Open: partner support for ZEC or a stablecoin route, fees on small amounts, KYC rules for buyers, and the regulation of QRIS for crypto purchases.

**Settle to bank automatically.** A merchant can choose to receive IDR instead of ZEC. Two designs are possible, and both have trade-offs:
- The merchant's wallet sends to the off-ramp partner on a schedule. This keeps Lunaz non-custodial, but the merchant must approve each transfer from their wallet.
- The invoice routes the payment to the merchant's account at the partner. This is automatic, but the merchant gives up the shielded receipt, and the partner sees each sale.
We will build the first design first and explain the privacy cost of the second one plainly.

**Wallet created in the browser.** "Generate a new wallet" in the dashboard. The 24-word recovery phrase is made in the browser with a small WebAssembly module and never leaves it. Only the viewing key goes to Lunaz. Research and a working prototype exist, and the key matches what ZODL derives from the same phrase.
Open: the 1.4 MB download, and how to help people store a recovery phrase safely.

**Payment links and invoices without code.** A merchant creates a payment link or a printable QR in the dashboard and shares it on WhatsApp, Instagram or a printed card. No API and no website needed.

**Receipts by email and WhatsApp.** A buyer can ask for a receipt. The receipt shows LUNAS, the amount and the shop name, and nothing that identifies the buyer's wallet.

### Later

**A cashier app for physical shops.** A small installable web app (PWA) for a tablet or phone at the counter, with prices in IDR and the LUNAS screen. The gateway already supports this. We removed the demo cashier to focus on online sales first.

**Plugins for online shops.** WooCommerce first, then Shopify and the Indonesian marketplace and storefront builders that allow external payment methods.

**Indonesian language.** The dashboard, checkout and docs in Bahasa Indonesia.

## Theme 2: Revenue

Lunaz earns only when a merchant gets paid. Monthly plans are replaced by per-payment fees.

### Now

**Fee inside the shielded payment.** Built. A ZIP-321 request with two recipients pays the merchant and Lunaz in one transaction. The fee is 0.5% (minimum 0.00001 ZEC) and the buyer pays it on top of the price. Lunaz still never holds the merchant's funds. If a wallet pays only the merchant part, the missing fee shows as outstanding on the dashboard Fees page, and the merchant settles it there in one payment.
Open: wallet support for payment requests with more than one recipient. ZODL must be tested first.

**App fee on cross-chain payments.** Done. Lunaz uses the 1Click `appFees` parameter to earn 0.75% of each swap: the 0.5% Lunaz fee plus 0.25%. The buyer pays it inside the deposit amount.

### Later

**Revenue share with partners.** Referral or revenue share from the off-ramp and on-ramp partners.

**Volume pricing.** Custom prices for large platforms and marketplaces.

## Theme 3: Marketplaces and platforms

### Next

**Create sellers through the API.** A platform creates and manages seller accounts with one platform key, instead of one dashboard signup per seller.

**Platform fee per order.** A marketplace adds its own fee as a second output in the same payment, so it earns on each sale without holding seller funds.

### Later

**Seller onboarding pages.** Hosted pages where a marketplace sends a new seller to connect a wallet or create one, with the platform's branding.

## Theme 4: Trust, security and reliability

### Now

**Password reset and email verification.** Through an email provider.

**Safer production defaults.** The service refuses to start with test-only options turned on.

### Next

**Two-factor sign-in.** An authenticator app (TOTP) first. Then "Pay to verify" as a second factor for sensitive actions: the merchant sends a tiny payment with a one-time code from their own wallet, and Lunaz confirms it with the wallet's outgoing viewing key.
Open: whether ZODL accepts zero-value requests with a pre-filled memo, and protection against an attacker who shows a victim a QR code.

**Chain reorganisations.** Detect a reorg, roll back the affected payments and correct the invoice status.

**Rate limits that survive restarts.** Webhook retries already survive a restart. Move the rate limits to the database too.

**Real client IP behind the proxy.** Read the trusted proxy header so the rate limits apply per client.

### Later

**Our own Zcash infrastructure.** Run our own Zebra node and lightwalletd (or Zaino) instead of public servers, with a fallback.

**PostgreSQL and more than one instance.** For uptime and more merchants.

**Status page, monitoring and an external security audit.**

## Theme 5: Developers

### Next

**Test mode.** A test API key that creates invoices you can mark as paid without real funds, for integration tests. A testnet build runs on pre-release Zcash libraries with the current testnet upgrade (NU7). It merges into the main build when the released libraries support that upgrade.

**OpenAPI description and SDKs.** An OpenAPI file, and small SDKs for JavaScript and Python with webhook signature checks built in.

**Webhook replay.** Send a past event again from the delivery log.

### Later

**API versioning and a public changelog.**

## Theme 6: Accounting and compliance

### Next

**Tax-ready reports.** Reports in IDR with the rate at the time of payment, ready for an accountant. Exports for local accounting tools.

**Legal basics.** Terms of service, a privacy policy, and advice from Indonesian counsel on the role of Lunaz next to licensed partners.

### Later

**Read-only access for accountants.** An accountant can sign in and see the ledger and exports, but cannot create keys or change settings.

## Theme 7: The Zcash ecosystem

These depend on work outside Lunaz. We follow them and adopt them when they are ready.

- **New network upgrades.** Keep the libraries current, and test each upgrade (for example NU7) before it activates.
- **Wallet connection.** If ZODL or another wallet adds a way to connect to web apps, offer "Sign in with your wallet".
- **Message signing for shielded addresses.** If an Orchard equivalent of ZIP-304 appears, use it for sign-in and proof of wallet ownership.
- **Shielded deposits at partners.** If off-ramp partners accept shielded deposits, withdrawals stop leaving the shielded pool.
