---
layout: post
title: "Understanding eToro: how the social trading platform actually works"
date: 2026-03-07
---

eToro went public in May 2025 at $52 per share. Nine months later, it trades around $33. The company is profitable, holds $1.3 billion in cash with no financial debt, and generates 34% free cash flow margins. Its signature product, CopyTrader, is patented and has no direct equivalent at scale. On paper, the setup looks straightforward. In practice, neither the valuation gap nor the product tells the story you'd expect.

This post walks through the business piece by piece, using eToro's SEC filings (20-F, F-1, 424B4) as primary sources.

## What does eToro do?

eToro is a retail investing platform that operates in 75 countries. Users can trade equities, ETFs, crypto, commodities, currencies, and CFDs (contracts for difference, a leveraged derivative popular outside the US). As of year-end 2025, the platform had 3.81 million funded accounts and $18.5 billion in assets under administration.

The company's differentiator is CopyTrader, a patented feature that lets users automatically replicate another trader's portfolio. When you copy someone, the system proportionally mirrors their entries, exits, position sizing, stop-losses, and take-profit levels. A "Pro Investor" program certifies experienced traders whose track records are displayed publicly, and copiers can allocate capital to replicate their strategies with a $1 minimum per copied position.

eToro was founded in 2007 in Israel by Yoni Assia (CEO) and Ronen Assia. It is incorporated in the British Virgin Islands and reports under IFRS as a foreign private issuer on Nasdaq.

## How does it make money?

This is where eToro's financials get confusing.

The company's 20-F reports total revenue of $13.8 billion for FY2025. That number is misleading. eToro operates as a principal in crypto trading: it buys crypto at market price, sells it to users at a markup, and reports both sides gross. Revenue from crypto sales was $12.975 billion, and the corresponding cost was $12.932 billion, leaving a net margin of roughly $43 million on crypto spot trading. The $13.8 billion headline tells you almost nothing about the size of the business.

Net revenue, total revenue minus crypto cost of revenue, was $906 million. That is the economically meaningful number.

Here is how it breaks down:

| Source | FY2025 ($M) | % of net revenue |
|--------|-------------|------------------|
| Trading spreads (equities, commodities, currencies) | 399 | 44% |
| Net interest income | 213 | 24% |
| Crypto (net of cost) | 167 | 18% |
| Currency conversion and other | 126 | 14% |

Net revenue grew from $595 million in 2022 to $906 million in 2025, a 15% compound annual growth rate.

Two things stand out in the revenue mix. First, trading spreads are the largest component but have been a shrinking share (from 66% in 2022 to 44% in 2025), while interest income has grown from 13% to 24%. Second, interest income of $213 million depends on the current elevated rate environment. If rates decline meaningfully, that 24% slice compresses.

## Does CopyTrader actually help users?

eToro's pitch is that CopyTrader democratizes access to experienced traders. A novice can allocate capital to a Pro Investor and automatically get their execution discipline, their risk management parameters, their entries and exits, all without needing to learn technical analysis or monitor markets.

The system faithfully replicates the copied trader's full plan. But it gives copiers one override: the ability to manually close any individual position early while the copied trader remains in it. Copiers cannot open new positions, re-enter closed ones, or modify stop-loss or take-profit levels. They can only exit early.

This creates what you might call an asymmetric intervention problem. Copiers absorb full downside, since losses run unless they manually intervene. But they can voluntarily cut upside short by panic-exiting winning positions before the copied trader's take-profit is reached. A strategy designed around a 1:3 risk/reward ratio degrades if copiers systematically exit at 1:1.5.

Peer-reviewed research on social trading platforms (Apesteguia, Heimer, and others) finds that exposure to leaderboards and top performer track records increases risk-taking and amplifies the "disposition effect," the well-documented tendency to sell winners early and hold losers. CopyTrader's early-exit valve is the exact mechanism through which this bias re-enters a system designed to remove it.

Per mandated regulatory risk disclosures, a majority of retail accounts lose money trading CFDs on eToro's platform. The figure varies by jurisdiction and reporting period, but it is lower than pure-play CFD competitors like Plus500 and IG Group. That difference likely reflects eToro's diversified product mix (users holding unleveraged spot crypto and equities buffer their CFD losses) rather than CopyTrader providing a behavioral shield. No public data exists on how frequently copiers exercise the early-exit override, or on the gap between Pro Investor displayed returns and copier actual returns.

CopyTrader is a genuine differentiator. It is patented, it creates switching costs through copy relationships and social graph stickiness, and it drives a flywheel where more users attract more content and more Pro Investors. But the claim that it solves retail trading's behavioral problems is unsupported by the available evidence. It is a distribution and engagement advantage, not a performance advantage for end users.

## What's going well?

eToro's financial trajectory over the past three years has been strong.

| Metric | FY2022 | FY2023 | FY2024 | FY2025 |
|--------|--------|--------|--------|--------|
| Net revenue ($M) | 595 | 582 | 824 | 906 |
| Operating margin | -32.7% | 5.4% | 30.4% | 29.2% |
| Net income ($M) | -215 | 15 | 192 | 216 |
| Free cash flow ($M) | 343 | 110 | 266 | 313 |
| ROIC | N/A | 3.1% | 27.4% | 20.2% |

The company swung from a $215 million loss in 2022 to $216 million in profit by 2025. Operating margins went from -33% to +29% as net revenue grew faster than operating expenses. This is operating leverage at work: R&D and G&A costs stayed relatively flat while revenue scaled.

The business model is capital-light. Capital expenditure was $5.5 million in FY2025, less than 1% of net revenue. Nearly all operating cash flow converts to free cash flow. FCF margin was 34.5%.

The balance sheet is a fortress. eToro holds $1.275 billion in cash and short-term investments with zero financial debt. At a hypothetical 50% revenue impairment, fixed costs of roughly $395 million per year give the company over 30 months of runway before needing outside capital.

Stock-based compensation, a common concern with recently public tech companies, has declined 87%, from $127 million in 2022 to $16 million in 2025. SBC as a percentage of operating cash flow dropped from 37% to 5%.

Management stability is a strength. CEO Yoni Assia has led the company since founding in 2007, a 19-year tenure. The CFO joined in 2019 and was previously on the 888 Holdings IPO team. The COO is a former Israeli Supervisor of Banks. Post-IPO, the board added Laura Unger (former SEC Commissioner) and Lior Shemesh (CFO of Wix) as independent directors.

Since the May 2025 IPO, management has delivered on every tracked commitment: US options trading launched, the Spaceship acquisition in Australia closed, a $250 million revolving credit facility was secured, securities lending launched in the UK and Europe, and the share repurchase program deployed $62 million at an average price of roughly $40 (below the $52 IPO price). The track record is clean, though the sample size is under one year.

## What are the risks?

**Revenue cyclicality.** This is the core risk. Quarterly Net Contribution (eToro's preferred measure of top-line economics) has ranged from $121 million (Q2 2023) to $252 million (Q4 2024), a 2x swing driven by retail trading sentiment and crypto market cycles. The company's 20-F acknowledges this directly: "our operating results have and will continue to fluctuate significantly from period to period."

**Crypto volatility.** Crypto contributed $155 million of $868 million in Net Contribution (18%) in 2025. But the quarterly pattern is extreme: $95 million in Q4 2024, then $26 million in Q4 2025, a 73% decline in a single year. Crypto spreads and volumes are tied to market cycles that eToro cannot control.

**Interest rate sensitivity.** Net interest income of $213 million represents 24% of net revenue. This grew from $78 million in 2022, driven entirely by the elevated rate environment. Management acknowledges the risk. If rates normalize to around 3%, modeling suggests operating margins would compress from roughly 30% to somewhere in the 24-28% range, survivable but meaningful.

**ASIC proceedings.** The Australian Securities and Investments Commission commenced proceedings against eToro in August 2023 regarding target market determinations for complex products. The outcome could include financial penalties and has the potential to trigger a class action lawsuit. The proceedings remain unresolved as of March 2026.

**Sanctioned shareholder.** SBT Venture Fund I, whose majority limited partner is a subsidiary of Sberbank of Russia, holds 3.76% of Class A shares. SBT is subject to US, UK, EU, and BVI sanctions and is restricted from voting, transferring shares, or receiving distributions. If sanctions are lifted, SBT would receive an additional 120,606 Class A shares and 2,630,552 Class B shares (which carry 10x voting rights). This creates an ownership overhang, compliance friction for institutional investors, and reputational risk. eToro has no control over when or whether this resolves.

**Governance gaps.** eToro is incorporated in the BVI and follows home-country governance practices that diverge from Nasdaq standards. The differences include lower quorum requirements (25% vs. 33.3%), no shareholder approval required for equity compensation plans or share issuances above 20%, and no proxy statement requirements. A dual-class share structure gives Class B shares 10 votes per share, though a 9.99% per-holder voting cap partially mitigates concentration risk.

**Competition.** Robinhood launched in the UK in late 2024 and is expanding across Europe. It has 24.8 million funded accounts (6.5x eToro's base), $4.8 billion in cash, and introduced copy trading through Robinhood Strategies in 2025. Revolut, with over 45 million users, is also expanding stock trading in eToro's core European markets. eToro's CopyTrader patent protects the specific implementation, not the concept of social trading.

## Why is it so cheap?

eToro trades at a significant discount to its US-listed peers.

| Company | Trailing P/E | Diluted EPS | Source |
|---------|-------------|-------------|--------|
| eToro | 14.4x | $2.27 | 20-F (FY2025) |
| Interactive Brokers | 30.7x | $2.22 | 10-K (FY2025) |
| Robinhood | 37.3x | $2.05 | 10-K (FY2025) |

All three EPS figures come directly from SEC filings. Prices as of early March 2026.

The US brokers trade at large premiums driven by scale, US regulatory advantages (payment for order flow for Robinhood, institutional/professional penetration for Interactive Brokers), and longer public track records. Those premiums are at least partially structural, and eToro is unlikely to close the full gap. But trading at less than half the multiple of the cheapest US peer raises the question of whether the discount is overdone.

Several identifiable overhangs contribute:

The Sberbank stake creates compliance friction. Institutional investors with ESG or sanctions-screening mandates cannot hold or are reluctant to hold a stock with a sanctioned Russian entity on the cap table. eToro's institutional ownership is roughly 20%, abnormally low for a company of its size and profitability. The ASIC lawsuit adds legal and reputational uncertainty. The BVI incorporation and governance exemptions reduce the protections institutional investors expect. The company has been public for less than a year, meaning many institutional mandates require a longer track record before initiating positions. And operating primarily in Europe, where regulators ban payment for order flow and cap CFD leverage, structurally limits margin potential compared to US brokers.

Some of these overhangs are structural and may never fully resolve: the BVI governance, the dual-class shares, the European regulatory environment, the inherent revenue cyclicality. Others could be temporary: the ASIC proceedings (approaching three years, resolution increasingly likely), the Sberbank stake (geopolitics-dependent), and the low institutional ownership (which may naturally increase as the company builds a public track record).

To put the valuation in context: eToro's market cap is roughly $2.7 billion. Net cash of $1.275 billion represents 47% of that market cap. Stripping out cash, the market is pricing the operating business at roughly $1.4 billion, or about 4.5x the $313 million in free cash flow generated in FY2025. Whether that is cheap depends on what you think normalized FCF looks like through a full market cycle, including periods of lower rates and weaker crypto sentiment. Using the peak-period $313 million overstates the case. Using the trough-period $110 million (FY2023) understates it. The truth is somewhere in between, and reasonable people will disagree on where.

## What to watch

These are the metrics and events that will determine which direction the business goes from here.

**Quarterly Net Contribution.** The single most important number. If it stays above $200 million per quarter, the business is healthy. If it falls below $150 million for two consecutive quarters, something has changed structurally.

**Operating margins through rate cuts.** Net interest income has been a buffer against trading cyclicality. As central banks cut rates, watch whether margins stay above 20-24%. If they do, the business is more resilient than the market gives it credit for. If they compress toward 15%, the cyclicality thesis is confirmed.

**ASIC resolution.** The Australian proceedings have been open since August 2023. A settlement that doesn't involve license revocation or penalties large enough to trigger cascading regulatory actions elsewhere would remove a meaningful overhang.

**Funded account growth.** Net adds were 310,000 in FY2025. Two consecutive quarters of negative net adds would indicate competitive pressure from Robinhood or Revolut is biting. The registered-to-funded conversion rate (roughly 10%, from 40 million registered to 3.8 million funded) is worth monitoring as well.

**FPI status test.** eToro's next test for foreign private issuer status is June 30, 2026. If US ownership exceeds 50%, the company would lose FPI status and face enhanced disclosure requirements (10-K/10-Q instead of 20-F, Section 16 insider trading reporting, proxy statements). This would increase compliance costs but also improve transparency.

**Buyback execution.** Management has a $250 million repurchase authorization and has been buying at prices well below the IPO. Whether they continue deploying at current levels, and at what prices, is a signal of how management views the stock's value.

