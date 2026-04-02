---
layout: post
title: "Polymarket is right about everything except your chances"
date: 2026-04-02
---

Polymarket, the largest prediction market in history, achieves 73-94% accuracy forecasting real-world events. It outperforms polls, pundits, and models. It called the 2024 presidential election when most polls showed a toss-up. It is valued at $9 billion and backed by the company that owns the New York Stock Exchange.

It is also a market where 70-84% of participants lose money, where 0.04% of addresses capture 71% of all gains, and where 14 of the top 20 most profitable accounts are automated trading bots.

Both of these things are true, and they are not a contradiction. They are the same mechanism.

## How it works

Polymarket is built on a blockchain (Polygon, an Ethereum layer-2 network), but you would not know it from using the product. You can sign up with a Google account, deposit money with a credit card, and start trading in minutes. If you already have a crypto wallet with USDC, the path is even shorter: connect the wallet, and you are trading. No account creation, no email, no personal information. There are no gas fees to pay and no tokens to buy. The platform handles transaction costs on your behalf through a relayer that submits trades to the blockchain for you. This is worth pausing on because it explains why Polymarket succeeded where earlier crypto prediction markets failed. Previous attempts like Augur required users to navigate wallet software, buy specific tokens, and pay transaction fees on every trade. Polymarket removed all of that friction. The blockchain is there for settlement security, but it is invisible to the user.

Every market is a yes-or-no question. You buy shares that pay $1.00 if you are right and $0.00 if you are wrong. Prices are set by an order book, the same mechanism stock exchanges use. When a buyer willing to pay $0.60 for a yes share meets a seller willing to take $0.40 for a no share, the system takes their combined dollar, creates one yes and one no token from a smart contract, and sends each person their respective share. There is no house setting odds. Prices move as traders buy and sell based on new information. You can sell your position at any time before the market resolves, making Polymarket function more like a financial exchange than a sportsbook.

The platform processed over $9 billion in trading volume in 2024. A single market on whether Trump or Harris would win attracted $3.7 billion in bets, though researchers at Paradigm later found that most Polymarket dashboards double-count volume by recording both sides of every trade, and a Columbia University study estimated that roughly 25% of volume is wash trading. After adjusting for both effects, real economic activity is significantly lower than headline figures, but Polymarket remains the largest prediction market ever built by a wide margin.

## The backstory

Shayne Coplan created Polymarket in 2020 from his apartment in New York during the pandemic. He was 22. Bloomberg now lists him as the world's youngest self-made billionaire. The company has raised approximately $2.3 billion, with the most consequential investor being Intercontinental Exchange, the company that owns the NYSE, which has committed roughly $2 billion. The former chairman of the CFTC sits on the advisory board. Nate Silver is an adviser.

The regulatory path was rough. The CFTC fined Polymarket $1.4 million in 2022 for operating an unregistered binary options platform. The FBI raided Coplan's apartment in November 2024 investigating whether the US geofence was intentionally weak. No charges were filed, and both the DOJ and CFTC investigations closed in July 2025. Polymarket then acquired a CFTC-regulated exchange for $112 million and received formal approval to re-enter the US market. The international platform is available in roughly 160 countries with no identity verification. It is blocked in about 30 countries including the US, where a separate regulated platform launched in December 2025, invite-only, with full KYC requirements.

For most of its life, Polymarket charged zero fees. That changed in 2026 with the introduction of taker fees across most categories, highest at 50/50 odds (about 1.8% on crypto markets, about 1% on politics, about 0.75% on sports). This matters for what comes next.

## The accuracy case

Polymarket's strongest selling point is that it produces better predictions than polls, pundits, or models. An analysis of resolved markets since 2023 found about 73% accuracy overall, outperforming traditional polls by 8 percentage points in political forecasting. Accuracy rises to roughly 90% a month before events and 94% just hours before resolution.

The mechanism is straightforward: people with information have a financial incentive to express it. If you know something the market does not, you can profit by trading on it. This pulls prices toward reality faster than surveys or expert panels.

The 2024 presidential election was the showcase. While most polls showed a tight race, Polymarket prices shifted decisively toward Trump in the final weeks. Polymarket was right. This cemented its reputation as a forecasting tool, drew mainstream media coverage, and helped drive its $9 billion valuation.

But accuracy as a forecasting tool and profitability as a trading venue are different questions.

## Who makes money

Approximately 70% of Polymarket's 1.7 million trading addresses have posted negative realized returns. A separate analysis using Dune Analytics data puts the figure at 84%, with only 16% of wallets in profit.

The distribution is extreme. A total of 668 addresses, 0.04% of all traders, captured roughly 71% of all realized gains. Most "profitable" traders earned between $0 and $1,000. On the loss side, 149 addresses lost more than $1 million each. Over 1.1 million addresses recorded losses between $0 and $1,000.

The most famous winner is a French trader known as Theo, who operated at least four accounts across 11 wallets. Chainalysis linked these to over $84 million in profits, primarily from betting on Trump in 2024. Theo reportedly commissioned custom polling through YouGov that asked swing state voters about their neighbors' voting preferences rather than their own, a technique designed to surface hidden preferences that standard polls miss.

## The bots

The real story is not the French whale. It is the bots. Fourteen of the top 20 most profitable wallets on Polymarket's leaderboard are automated trading systems.

The most documented is [0x8dxd](https://polymarket.com/@0x8dxd): $313 in starting capital, $2.38 million in cumulative profit, 33,951 trades, 98% win rate. The strategy was not prediction. It was speed. The bot monitored Bitcoin prices on Binance and Coinbase and exploited the 2-3 second delay before Polymarket's order book adjusted. When Bitcoin dropped on Binance, the bot bought "no" on Polymarket's "Bitcoin up in 15 minutes?" market before anyone else could reprice. It was reading a price that had already moved and trading faster than everyone else.

A second account, [k9Q2mX4L8A7ZP3R](https://polymarket.com/@k9q2mx4l8a7zp3r), followed a similar pattern: 54,356 trades, $208.9 million in volume, $1.72 million in profit. Its profit curve went flat in mid-March 2026, likely because Polymarket's new fees made its thin-margin strategy unprofitable. The average arbitrage window compressed from 12.3 seconds in 2024 to 2.7 seconds in 2025. Professional setups colocate servers at data centers in London, near the AWS infrastructure where Polymarket's matching engine runs, achieving sub-millisecond latency.

Polymarket introduced its fee structure specifically to kill latency arbitrage, and it appears to be working. Several prominent speed bots stopped trading as fees expanded in March 2026. But the fees only addressed one type of edge. An account called [ilovecircle](https://polymarket.com/@ilovecircle) earned $2.24 million with only 1,347 trades using machine learning models to estimate probabilities across sports, politics, and crypto, trading when market prices diverged from its model's estimates. Its 74% accuracy rate is lower than a latency bot's 98%, but its average trade size was $39,500, roughly seven times larger, because it was trading on genuine informational edge rather than execution speed. Fees do not touch this kind of advantage.

And behind both the speed bots and the AI bots sit the market makers: accounts that post buy and sell orders on both sides of every market, capturing the spread on each trade and collecting Polymarket's maker rebates. Market makers are the quiet, steady-state winners of any exchange. Polymarket's fee structure, which charges takers and pays makers, is designed to attract them. As latency bots exit, the spread-capturing market makers are increasingly the ones on the other side of retail trades.

## Why most participants lose

If Polymarket is just people trading against each other with no house edge, shouldn't the profits and losses be roughly evenly distributed?

No. Total profits equal total losses. But the distribution depends entirely on who has edge. Consider 1,000 people at a poker table. After 10,000 hands, the total money has not changed. But 50 people hold most of it, and 950 have less than they started with. The money did not disappear. It concentrated. Skill differences do not just create winners and losers. They create a distribution where the skilled few extract from the unskilled many, repeatedly, because the skilled participants play more hands, at larger stakes, with a persistent edge that compounds.

On Polymarket, the skill gap is extreme. A bot executes 30,000 trades with a 98% win rate. A casual participant makes 20 trades based on intuition. The casual participant's losses are small individually, but they are funding someone else's consistency. Multiply that person by a hundred thousand and you get 668 addresses holding 71% of all gains.

Human psychology compounds the problem. People trade when they have no edge, which is equivalent to volunteering for the wrong side of someone else's informed trade. They sell winners too early and hold losers too long. They overpay for long shots because the potential payoff feels exciting. Polymarket's own data shows this: low-probability events are systematically overpriced. These biases do not cancel out. They do not make you randomly wrong. They make you systematically wrong.

The stock market comparison matters. Barber and Odean at UC Berkeley found that 70-90% of day traders lose money, strikingly similar to Polymarket. But the stock market has a critical difference: the underlying assets create value. Companies earn revenue, grow, and return capital. Even a completely uninformed investor who buys an index fund makes money over time, capturing the value the economy creates. The stock market is positive-sum in aggregate.

Prediction markets are pure zero-sum. No dividends, no earnings growth, no underlying value creation. Every dollar of profit requires a dollar of loss from someone else. There is no index fund equivalent. Being mediocre in the stock market still works if you buy and hold broadly enough. Being mediocre in a prediction market means you are the liquidity that the informed participants extract from.

## The accuracy paradox

This leads to the core tension. The better a prediction market is at its stated purpose, the worse it is for uninformed participants.

Polymarket achieves 73-94% forecasting accuracy because informed traders move prices toward reality. They move prices toward reality by taking money from people whose trades move prices away from reality. The uninformed participants are not incidental to the market's accuracy. They are the fuel for it. Their losses are the mechanism by which the market becomes correct.

This is not a broken system. It is the system working as designed. A prediction market aggregates information by financially rewarding those who have it and financially punishing those who don't. The 70-84% who lose are the cost of the market's accuracy. Whether that trade-off is acceptable depends on whether you are looking at Polymarket as a public forecasting tool or as a place where you put your money.

The forecasting tool is real, and the data supports paying attention to its prices. The trading venue is also real. If you are considering putting money in, ask yourself what edge you have that the bots and the quants do not. If you cannot answer that, you are not the trader. You are the trade.
