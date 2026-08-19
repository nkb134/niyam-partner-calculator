# Niyam Partner Earnings Estimator

An interactive calculator for trading institutes, sub-brokers and channel partners to
estimate monthly and annual commission earnings from traders they onboard to Niyam Exchange.

**Live:** https://nkb134.github.io/niyam-partner-calculator/

## What it does

The page has two tabs.

**Earnings estimator.** The partner enters two numbers only they know — how many traders they
can onboard, and the average funds those traders hold. Everything else is a pre-filled,
editable assumption.

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

Because gross fees reduce to `volume × bps`, the volume needed to hit any commission target
is `target ÷ (bps × partner share)` — independent of leverage, funds or trader count. Those
only change how the volume is reached.

**Milestone rewards.** Monthly trading volume goals that pay a fixed reward on top of the
standard commission. The reward stacks; it does not replace commission.

| Monthly trading goal | Standard commission @ 30% | Milestone reward | Total, % of gross |
|---|---|---|---|
| $1M | ₹11,400 | ₹2,000 | 35.3% |
| $10M | ₹1,14,000 | ₹35,000 | 39.2% |
| $50M | ₹5,70,000 | ₹3,50,000 | 48.4% |

Figures shown at 4 bps and ₹95/$. The table is not hardcoded — it recomputes from the fee
rate and commission share set on the estimator tab, plus the conversion rate on the rewards
tab. The last column is therefore derived, not fixed: because rewards are flat rupee amounts
while gross fees scale with the fee rate, the percentage **rises as the fee rate falls**. At
2 bps the same tiers read 40.5 / 48.4 / 66.8%.

## Current defaults

| Parameter | Default | Status |
|---|---|---|
| Trading fee | 4 bps on notional | **Unconfirmed** — see note below |
| Discount to traders | 10% | Confirmed |
| Partner commission share | 30% of gross | Enterprise / institutional tier |
| Milestone rewards | ₹2,000 / ₹35,000 / ₹3,50,000 | **Unconfirmed** — set by hand, see note |
| INR per USD | 95 | Assumption — editable on the rewards tab |
| Active trader rate | 60% | Assumption |
| Round trips / trader / month | 20 | Assumption |
| Average leverage | 10× | Assumption |
| Funds deployed as margin | 60% | Assumption |

There is no flat or minimum per-order fee. Fees are purely bps on notional.

> **Before publishing:** the fee rate, commission share and milestone rewards are not locked.
> Do not distribute this URL to partners until all three are signed off.
>
> The reward amounts are fixed figures chosen by hand, not derived from the tier rates. A
> 5 / 10 / 20% uplift on gross at 4 bps and ₹95/$ would be ₹1,900 / ₹38,000 / ₹3,80,000,
> or ₹2,000 / ₹40,000 / ₹3,80,000 rounded up to the nearest 500 / 5,000. Against that
> rounded basis the middle and top tiers as configured pay ₹5,000 and ₹30,000 less.
> Confirm which basis governs before the tiers go into a partner agreement.

## Editing

Single self-contained `index.html`. No build step, no dependencies, no external assets —
the logo is embedded as base64 and fonts fall back to system faces if Google Fonts is
unreachable. Open it in a browser to test, commit to `main` to deploy.

To change estimator defaults, edit the `value=` attributes on the inputs and the matching
`DEFAULTS` / `PRESETS` objects in the script block at the bottom.

To change the reward tiers, edit the `TIERS` array in the same script block. Each entry is
`{vol, reward}` — `vol` is the monthly volume goal in USD, `reward` the fixed payout in INR.
Adding or removing an entry adds or removes a table row; nothing else needs to change. The
default conversion rate lives on the `#fx` input.

Both tabs are driven by one `calc()` call, so the estimator inputs and the rewards table
stay consistent with each other.

## Compliance

The tool states that figures are illustrative estimates, not guarantees, and that actual
earnings depend on trader behaviour. It makes no claim about returns for traders and gives
no investment advice. Fees shown exclude GST. Commission payouts and milestone rewards are
stated as subject to applicable taxes, withholding and the partner agreement.

Any change to the disclaimer text in the page footer, or to the note under the rewards
table, should go through compliance review before it ships.
