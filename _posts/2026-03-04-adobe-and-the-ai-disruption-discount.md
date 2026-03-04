---
layout: post
title: "Adobe and the AI disruption discount"
date: 2026-03-04
---

Adobe's stock is down roughly 20% over the past three years while Microsoft gained 50% and Shopify nearly tripled. The market has re-rated Adobe as an AI disruption victim. I spent time digging through five years of SEC filings to understand whether that narrative holds up.

## The business behind the narrative

Adobe generated $23.8B in revenue in FY2025, up from $15.8B in FY2021. That's a 10.8% compound annual growth rate sustained through rate hikes, a failed $20B acquisition, and the generative AI upheaval that supposedly threatens the company's existence.

The revenue is 96% subscription. Gross margins sit at 89%. Free cash flow was $9.85B on a 41% FCF margin. Return on invested capital hit 63%. Capital expenditure is under 1% of revenue. By every traditional quality metric, this is an exceptional business.

The subscription model matters more than it looks in a table. Adobe collects cash upfront (deferred revenue was $7.0B at year-end), pays suppliers later (negative cash conversion cycle of -24 days), and converts reported earnings into cash at 1.38x. Operating cash flow has exceeded net income every year for the past five.

## Where the money goes

Adobe spent $35.7B on share buybacks over five years, reducing diluted shares from 481M to 427M (an 11% reduction). This more than offset stock-based compensation dilution, which ran at 19.8% of operating cash flow, above the threshold I flag as concerning.

The buyback math has a problem, though. Adobe issued $4B in new debt across FY2024-2025 to fund repurchases. In FY2025 alone, buyback spending of $11.3B exceeded annual free cash flow of $9.85B. And the timing was poor: the company repurchased shares at an average price of roughly $543 in Q2 FY2024 and ~$367 for full-year FY2025. The stock now trades 40%+ below those levels. That's real value destruction for shareholders.

On the other hand, at current prices the math flips. The unadjusted FCF yield is about 8%; adjusted for stock-based compensation (subtracting the $1.94B SBC from FCF), it's above 7%. Either way, every repurchase dollar is far more accretive than at peak valuations.

R&D investment has been consistent at ~18% of revenue, growing from $2.5B to $4.3B over five years. That spending funded the organic development of Firefly and AI integration across Creative Cloud. Adobe made essentially no acquisitions after FY2021 (the only notable M&A event was the failed Figma deal, which cost $1B in termination fees and returned nothing).

## The AI threat, examined specifically

AI mentions in Adobe's own risk factors grew from 6 (FY2021) to 49 (FY2025). Management is clearly worried. But the specifics of the competitive dynamics are more nuanced than "AI kills Photoshop."

Start with the cost structure. Traditional software has near-zero marginal cost per use. AI features break this. Every Firefly generation costs real GPU compute. Standard image inference runs $0.001-$0.01 per generation, but Adobe's paid plans include unlimited standard generations at zero marginal revenue. Firefly Video requires roughly 100x the compute per generation, which is why Adobe gates it behind premium credits.

This creates a genuine margin risk. The 89% gross margin is a software margin, not an AI margin. If AI usage scales faster than pricing offsets, margins compress.

Now look at the competitive landscape. The bear case assumes Adobe's competitors are somehow immune to this same problem. They aren't.

Figma's AI is strictly cloud-based, relying on third-party models (including OpenAI). They enforce a credit system with hard limits (3,000 for Pro seats) and charge ~$0.03 per extra credit. Midjourney is purely cloud-hosted on Google Cloud. Canva's generative core (Leonardo.ai "Phoenix") is cloud-bound. Every competitor faces the same inference cost economics. None of them have solved the unit economics problem Adobe is wrestling with.

Adobe has one structural advantage its competitors lack: IP indemnification. Firefly is trained on Adobe Stock, and Adobe contractually assumes legal defense for corporate IP infringement. For enterprise customers shipping creative assets at scale, that legal cover is a purchasing requirement, not a nice-to-have.

A caveat worth noting: a Bloomberg investigation in 2024 found ~5% of training data included synthetic Midjourney outputs uploaded to Adobe Stock. Because Adobe licensed those uploads from contributors, the indemnification structure remains contractually intact. But it complicates the "clean training data" narrative.

## The edge AI resolution

The margin compression from AI inference costs may be a transitional problem, not a permanent one.

On July 9, 2025, JEDEC published the LPDDR6 specification, pushing memory data rates to 14.4 Gbps. Combined with 2026-2027 NPU architectures (Intel Nova Lake NPU6, Snapdragon X2 Elite at 80 TOPS), the memory bandwidth bottleneck that currently prevents running diffusion models locally starts to ease.

Current laptop unified memory (LPDDR5X) hits 135-273 GB/s, which limits local model size. Discrete GPUs like the RTX 4090 hit 1,008 GB/s. LPDDR6 narrows that gap.

The practical implication: Adobe could route 50-70% of lightweight generative tasks (inpainting, masking, background removal) to local NPUs by 2027-2028. This shifts compute costs from Adobe's cloud infrastructure to the consumer's hardware. Margin problem becomes someone else's problem.

Competitors that are purely cloud-hosted (Midjourney, Figma's AI layer) cannot make this shift. Adobe has decades of experience with local application architecture and already ships desktop software that can leverage local hardware.

## The moat under stress

The standard moat analysis for Adobe is straightforward: proprietary file formats (PSD, AI, INDD), professional workflows built around Adobe tools, enterprise platform integrations, brand recognition ("Photoshop" is a verb), and scale ($4.3B in annual R&D).

What's less discussed is the Adobe Fonts lock-in. Canceling Creative Cloud severs Adobe Fonts access, which instantly breaks live client websites that depend on those fonts. For agencies and studios, this is a practical barrier to switching that has nothing to do with whether Photoshop is the best tool.

The enterprise plumbing is another layer. Adobe Experience Manager and GenStudio own the "Content Supply Chain," integrating AI generation with compliance workflows and multi-channel deployment. An enterprise that has built its content pipeline on Adobe's stack faces a multi-year migration to replace it. AI tools that generate images don't touch this layer.

The vulnerable segment is the mid-market and casual creative tier. Microsoft Copilot Designer has distribution advantage through 365 that could erode Adobe's position with non-professional users. Canva's Affinity V3 suite runs local NPU tasks for utility AI (object selection) while gating heavy generation behind a cloud subscription. For the casual user who does occasional creative work, "good enough" alternatives are real and growing.

The professional tier, where Photoshop, Premiere Pro, and After Effects operate, is more defensible. Feature parity requires decades of engineering in color science, non-destructive editing, and integration with professional production pipelines. No AI startup has replicated this, and the switching costs are measured in years of workflow retraining.

## The regulatory overhang

The FTC filed suit against Adobe in June 2024, alleging that the "Annual, Paid Monthly" subscription plans use dark patterns to prevent cancellation. A class action followed in August 2025. Internal documents surfaced describing the 50% early termination fee as "a bit like heroin for Adobe."

The financial exposure from the early termination fee itself is small (less than 0.5% of revenue). The real risk is that forced changes to cancellation practices increase churn across the subscription base.

There's a material development most coverage has missed: on July 8, 2025, the 8th Circuit Court of Appeals vacated the FTC's sweeping "Click-to-Cancel" rule. While Adobe still faces its specific lawsuit and potential UI mandates, the systemic risk of a federally mandated mass-churn event across the entire subscription software industry was removed.

## What I'm watching

Adobe shifted its primary guidance metric from "Digital Media ARR" to "Total Adobe ARR" ($25.2B exiting FY2025), arguing that AI features now cross-pollinate between segments. Digital Media ARR ($19.2B) is still reported supplementally. This is worth monitoring closely. When companies change their headline metric, sometimes it's legitimate (the business evolved) and sometimes it's obfuscation (the old metric was about to look bad). The FY2025 10-K referred to ARR in past tense, which caught my attention.

The Q1 FY2026 earnings (March 2026) are the first real test of whether AI features are driving tier upgrades. Adobe launched Creative Cloud Pro at $69.99/month and standalone Firefly Premium at $199/month. If these tiers drive Net New ARR and ARPU higher without increasing churn, the "AI victim" narrative breaks. If they don't, the market's skepticism is justified.

Gross margin trends will tell the inference cost story before management discusses it. A 200-400bps compression is absorbable. A decline below 85% TTM would signal that AI inference costs are structurally impairing the business model before edge AI hardware can offset them.

CEO Shantanu Narayen has led Adobe for 17+ years, overseeing the subscription transformation and navigating the company into the AI era. His departure without a succession plan would change the thesis. The resignation of CSO Scott Belsky, announced January 2025 and effective March 2025, is a minor watch item.

## The valuation disconnect

I won't call specific prices, but the math is worth stating. A reverse discounted cash flow analysis (10% discount rate, 2% terminal growth) implies the market is pricing Adobe for roughly 0-2% perpetual FCF growth. The company has compounded FCF at 9.4% annually for five years. Either the market is right that AI will break the growth engine, or Adobe is meaningfully mispriced.

The forward non-GAAP P/E sits at roughly 11x. Adobe's 10-year historical average on the same basis is in the mid-40s. The stock is trading at a ~66% discount to its own historical norm and sits at a decade-low valuation floor.

That discount could be correct. AI could commoditize creative tools, the FTC could force structural changes to the subscription model, and margins could compress permanently. But the filing evidence from the past five years tells a story of a company that has grown through multiple disruptions, generates cash far in excess of reported earnings, and is investing heavily in the transition. The market is pricing in a future where none of that matters.

Maybe the market is right. But the gap between what the filings show and what the market implies is wide enough to be interesting.
