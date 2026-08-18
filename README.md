# Niyam Partner Earnings Estimator

An interactive calculator for trading institutes, sub-brokers and channel partners to
estimate monthly and annual commission earnings from traders they onboard to Niyam Exchange.

**Live:** https://<your-github-username>.github.io/niyam-partner-calculator/

## What it does

The partner enters two numbers only they know — how many traders they can onboard, and the
average funds those traders hold. Everything else is a pre-filled, editable assumption.

The calculation chain:

```
Position size   = funds × margin deployed % × leverage
Orders / month  = active traders × round trips × 2      (entry + exit)
Volume          = position size × orders
Gross fees      = position size × bps × orders
Commission      = gross fees × partner share
```

Commission is calculated on **gross** fees, before the discount traders receive.
Niyam funds that discount; it does not reduce the partner payout.

## Current defaults

| Parameter | Default | Status |
|---|---|---|
| Trading fee | 4 bps on notional | **Unconfirmed** — see note below |
| Discount to traders | 10% | Confirmed |
| Partner commission share | 20% of gross | **Unconfirmed** for enterprise tier |
| Active trader rate | 60% | Assumption |
| Round trips / trader / month | 20 | Assumption |
| Average leverage | 10× | Assumption |
| Funds deployed as margin | 60% | Assumption |

There is no flat or minimum per-order fee. Fees are purely bps on notional.

> **Before publishing:** the fee rate and commission share are not locked. Do not
> distribute this URL to partners until both are signed off.

## Editing

Single self-contained `index.html`. No build step, no dependencies, no external assets —
the logo is embedded as base64 and fonts fall back to system faces if Google Fonts is
unreachable. Open it in a browser to test, commit to `main` to deploy.

To change defaults, edit the `value=` attributes on the inputs and the matching
`DEFAULTS` / `PRESETS` objects in the script block at the bottom.

## Compliance

The tool states that figures are illustrative estimates, not guarantees, and that actual
earnings depend on trader behaviour. It makes no claim about returns for traders and gives
no investment advice. Fees shown exclude GST. Commission payouts are stated as subject to
applicable taxes, withholding and the partner agreement.

Any change to the disclaimer text in the page footer should go through compliance review
before it ships.
