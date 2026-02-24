---
layout: post
title: "LLM scaling laws and the $680 billion question"
date: 2026-02-24
---

Everyone building or investing in AI is making a bet on the same thing: that models will keep getting better. Scaling laws are the mathematical framework for evaluating that bet. Here's what they actually say.

## How models improve: power laws

When you make a language model bigger or train it on more data, its prediction error (loss) drops following a power law:

```
L = a * x^(-p)
```

On a log-log plot, this is a straight line. The exponent `p` tells you how fast improvement comes. The catch: `p` is small, which means diminishing returns are baked into the math.

[Kaplan et al. (2020)](https://arxiv.org/abs/2001.08361) at OpenAI measured the exponents. For model size, `p ≈ 0.076`. That means 10x more parameters buys you about a 16% reduction in loss. The next 10x buys another 16% of the remaining gap. Each step closes 16% of the distance to a floor you never reach.

That floor is the irreducible loss: the inherent unpredictability of language. People make unpredictable word choices and creative decisions that no model can predict. This floor is estimated at roughly 1.0-1.5 nats/token for English.

[Hoffmann et al. (2022)](https://arxiv.org/abs/2203.15556) at DeepMind refined the picture with the Chinchilla scaling laws. They found that model size and training data should scale together, about 20 tokens per parameter for compute-optimal training. This showed GPT-3 was severely undertrained (1.7 tokens/param vs. the optimal 20).

An important caveat: 20 tokens/param minimizes *training* cost, not total cost. For models serving millions of queries, it's cheaper to overtrain a smaller model. Llama 3 8B trained at 1,875 tokens/param. By 2026, frontier models routinely train at 200+ tokens/param.

## The data wall

The usable high-quality text on the internet is estimated at 10-20 trillion tokens after deduplication and filtering. The largest known training set is around 18T tokens (Qwen 2.5). At 20 tokens/param, that optimally supports a ~900B parameter model.

Scaling further requires synthetic data (which may not have the same statistical properties), multi-epoch training (its own diminishing returns), or new modalities like video and audio.

## Inference-time compute: the new axis

As pretraining approaches diminishing returns, a second scaling axis opened up: spending more compute at inference time. Instead of one forward pass, the model thinks longer.

The [o1/o3 family](https://cameronrwolfe.substack.com/p/llm-scaling-laws) showed this works. Performance follows its own power law against test-time compute, largely independent of model size. o3 can spend up to 172x more compute on hard problems versus easy ones.

The simplest technique is chain-of-thought: the model writes out reasoning steps before answering. Accuracy scales log-linearly with the number of reasoning tokens. Best-of-N sampling generates multiple answers and picks the best, with error decaying as a power law in N (exponent ~0.3-0.5). Process reward models score each reasoning step rather than the final answer, achieving [4x better compute efficiency](https://arxiv.org/abs/2408.03314) than naive sampling. At the far end, latent space reasoning feeds hidden states back as inputs instead of generating tokens, letting a 3.5B model match 50B-parameter performance on reasoning benchmarks.

But inference-time compute has the same diminishing returns shape. Each 10x more samples buys roughly 2-3x less error.

## The verifier ceiling

Here's the real bottleneck: when you generate N candidate answers, something has to pick the best one. That's the verifier. Verifier quality sets the ceiling on how far inference-time scaling can go.

For math and code, verification is easy. Run the code, check the proof. That's why reasoning models work best in these domains. The verifier is perfect, so there's no ceiling.

For everything else, the verifier is a neural network with its own errors. [Gao et al. (2023)](https://arxiv.org/abs/2210.10760) showed that optimizing against an imperfect verifier follows a predictable pattern:

```
R_gold(d) ≈ alpha * d - beta * d^2
```

Performance initially rises, peaks, then declines as the model learns to exploit the verifier's blind spots rather than actually improve. More compute eventually makes things worse. Bigger verifiers push the peak further out but never eliminate it. Verifiers are themselves neural networks subject to the same scaling limits. It's diminishing returns all the way down.

## So, have we peaked?

Not yet, but the ceiling is getting closer on every axis.

Pretraining is approaching steep diminishing returns. High-quality data is running low, and each 10x in compute yields a smaller improvement. Inference-time compute still has real gains, but the same power-law shape applies, with the ceiling set by verifier quality. Verifier quality itself is improving but subject to its own limits. The escape hatch is formal verification (code execution, theorem provers), where the ceiling disappears. In open-ended tasks like strategy and creative work, the ceiling is real.

## What this means for AGI

The scaling laws don't rule out AGI, but they constrain the path.

Against the case: three layers of diminishing returns stack on top of each other (pretraining, inference, verification). A [2025 survey of 475 AI researchers](https://aireapps.com/articles/why-might-the-llm-market-not-achieve-agi/) found 76% think scaling current approaches is unlikely to produce AGI. The [ARC-AGI benchmark](https://arcprize.org/) for fluid intelligence remains hard. o3 reached ~88% but at enormous cost per task.

For the case: [Othello-GPT](https://arxiv.org/abs/2210.13382) showed language models can learn genuine world models from text alone, going beyond surface statistics. RL training (DeepSeek-R1) follows its own scaling curve, partially independent of pretraining. Scalable oversight research suggests models can evaluate outputs they can't generate. And hardware is still improving along the V100 → Blackwell progression.

The honest take: the math says the current recipe has predictable limits. Reaching AGI likely requires either discovering those limits are further away than they appear, or finding qualitatively new approaches.

## The $680 billion question

This is where the math meets the money.

The Big Five hyperscalers (Amazon, Alphabet, Microsoft, Meta, Oracle) have guided for combined capex of $640-720 billion in 2026 (midpoint ~$680B), up ~78% from ~$381B in 2025. About 75% goes to AI infrastructure. Capital intensity has reached levels unprecedented for tech companies: Oracle at ~87% of revenue, Meta at ~54-62%, Microsoft and Alphabet around 45%, and Amazon at 25-28%. Historical norm is 10-15%.

Against ~$500B+ in AI-directed capex, direct AI service revenue remains a fraction. Microsoft's AI business runs at $13B/year, the cleanest disclosed figure. Extrapolating across all five, total direct AI revenue is plausibly $40-60B, a capex-to-revenue ratio of roughly 8:1 to 12:1.

These companies are no longer self-funding the buildout. In 2025, the Big Five issued $121B in debt, 4x the five-year average. Morgan Stanley expects hyperscaler borrowing to exceed $400B in 2026. Amazon is projected to run negative free cash flow.

Sam Altman: *"Are we in a phase where investors as a whole are overexcited about AI? My opinion is yes. Is AI the most important thing to happen in a very long time? My opinion is also yes."*

The capex isn't pricing in AGI. It's pricing in sustained exponential improvement that translates to massive revenue growth, something like AI that reliably automates 30-50% of knowledge work within 5 years. The scaling laws suggest that will be harder and slower than the investment timeline assumes.

The most likely outcome is a messy middle. AI proves genuinely useful, revenue grows substantially but not fast enough to justify peak capex levels, some spending gets written off, valuations correct, and the technology continues developing on a longer timeline than investors priced in.

[GMO's analysis](https://www.gmo.com/americas/research-library/valuing-ai-extreme-bubble-new-golden-era-or-both_viewpoints/) maps current AI spending onto eight prior technology bubbles. The pattern repeats: the technology is real, but investment gets ahead of the revenue timeline. In the 1840s railway mania, the late 1990s telecom boom, and the 2000 dot-com crash, the technology eventually delivered transformative value, but the companies that built the infrastructure often didn't survive to benefit.

The investment was right. The timing was wrong.
