---
layout: post
title: "Truth-seeking with Claude Code"
date: 2026-04-24
---

I built a Claude Code skill called truth-seeking. It takes a thesis I care about, runs it through a Gemini fact-check, a Codex adversarial critique, and a Claude steelman defense, then hands me a short summary and asks what I want to look at more closely. The raw reviewer outputs stay in context but do not print by default; I pull specific pieces when I ask.

This post is about the reasoning behind the design, not the tooling alone. The load-bearing question is when cross-model review is actually worth the time, and the three failure modes the practice slides into when I am not careful.

## Why self-review is not enough

Single-model self-review has a documented failure mode. Stronger language models struggle to recognize when they are wrong in their own outputs, a self-preference effect where the harmful component scales with model size ([arXiv 2504.03846](https://arxiv.org/abs/2504.03846)). Running the same model twice does not add independence; it adds repetition. And even ensembles of multiple different models can converge on the same wrong answer when all members find the error plausible, a failure mode observed across five model families in code-generation benchmarks ([arXiv 2510.21513](https://arxiv.org/abs/2510.21513)). (arXiv is the open-access preprint server where AI research is typically posted; most formal publication in the field happens through conferences like NeurIPS, ICML, and ICLR.)

So the principle is clear. Single-model self-review has real blind spots, and rerunning does not fix them. What is much less clear is whether any particular combination of AI reviewers catches more than any other combination. Strong evidence for the first claim, thin evidence for the second. The skill reflects both: commit to the discipline of using more than one reviewer, and stay open to swapping specific reviewers as evidence accumulates.

The next question is what "more than one reviewer" actually means in practice. Most conversations about cross-model review get confused here.

## Three dimensions of independence

"Cross-model review" usually bundles three different things, each with its own evidence and its own cost.

The first is context independence. Run the review in a fresh session, isolated from the main conversation, so the reviewer does not carry whatever bias the main conversation developed. A sub-agent, meaning a Claude instance dispatched into a fresh context with a specific role so it works on the task without being biased by the main conversation, delivers this for free. You do not need a different model to get fresh eyes; you just need a fresh session.

The second is model independence. A different architecture and different training data means partially uncorrelated failure modes. When your main Claude confidently hallucinates something, a Claude sub-agent in fresh context likely hallucinates the same thing, because they share training. A different model family (OpenAI's Codex, Google's Gemini) is how you get failure modes that do not overlap. This is what cross-family review, running the same content through a different AI model family rather than another Claude instance, actually buys: not necessarily a better reviewer, but a differently wrong one.

The third is tool and grounding independence. Different retrieval backends, different source indexes, different grounding mechanisms. Gemini's native Google-index grounding is the strongest version of this. The model checks its output against live web sources it retrieves in real time rather than relying only on training data, and cites each claim with character-offset indexes into the page it quotes. Claude's WebSearch provides a partial version; Claude's WebFetch pipes pages through a summarizer with a 100 KB truncation and a 125-character quote limit (figures from community reverse-engineering of the Claude Code system prompt rather than Anthropic's public API documentation), a real fidelity loss on long sources. Codex has a web search tool too. But the skill only uses Codex for adversarial critique, not fact-checking, so its grounding does not come into the comparison.

Most "cross-model review is better" claims bundle all three dimensions together and treat the bundle as one thing. That conflation is where the conversation goes wrong. The dimensions have different evidentiary standing, different costs, and different workloads they suit. Keeping them separate is what lets you decide which parts of the bundle are actually worth the tooling.

## Three layers, honestly ranked

Given the decomposition, rank the layers honestly.

Multi-context review is the load-bearing one. Fresh eyes work across many domains for the same reason: the person who produced the work cannot see it the way someone who did not produce it can. Every editorial workflow, every design review, every peer-review process rests on this. If you do nothing else, run a fresh-context pass before you ship. This is the cheapest layer and the one with the strongest track record outside AI.

Multi-model review is probably additive and probably smaller than people claim. The mechanism is sound: different training lineage means partially uncorrelated failure modes, so a second model catches things the first model missed. But the empirical evidence for any specific two-model setup beating a single model with fresh context and an adversarial prompt is not established. Run it when the stakes justify the cost. Skip it when they do not.

Multi-tool review is the narrowest layer. The one thing Gemini's grounded search reliably does better than Claude's WebSearch is long-passage fact-checking. For verifying a specific number on a benchmark leaderboard, either works. For verifying whether a quoted paragraph faithfully represents a long source, Gemini's offset-indexed grounding is meaningfully tighter. If your fact-checks are mostly atomic, this layer adds little.

These ranks come with revision triggers, not pre-registered disconfirmation rules. Drop the multi-context layer if self-review catches the same things a fresh-context pass catches on three consecutive reviews. Drop the multi-model layer if a fresh-context same-model critique catches the same patterns as cross-family on three consecutive reviews. Drop the multi-tool layer if Claude's WebSearch matches Gemini's grounding fidelity on three consecutive long-passage checks. Log what you observe. Drop a layer when its signal goes to zero.

## Three failure modes of cross-model review itself

Cross-model review has its own failure modes. Using it without noticing them is worse than not using it at all. Three patterns, all lived.

The first is recency. After reading the fact-check, then the critique, then the steelman, whichever arrived most recently tends to shape your conclusion. Before deciding, ask yourself: if I ran the opposite framing, would my conclusion flip? If yes, you do not have a conclusion; you have an anchor.

The second is that adversarial prompts always find fault. If you ask a reviewer to attack an argument, it will. That is the tool working, not evidence that the argument broke. Adversarial critique is hypothesis-generation, not verdict-delivery. It surfaces patterns to weigh, not answers to accept.

The third is that every reviewer smuggles in its own calibration bias. Codex is tuned for rigor, so it demands evidence no practical decision ever has, like randomized controlled trials (formal studies where participants are randomly assigned to groups so cause and effect can be measured). Gemini is grounded in web search, so it anchors on whatever Google returns, which may be AI-generated content as often as primary sources. Claude's self-review favors its own outputs. Three biases, not zero, and the workflow breaks if you treat any one of them as neutral truth.

Here is the second pattern on my own draft. An earlier version of this post argued that Codex reviews code better than Claude and Gemini fact-checks better than Claude. When the benchmarks did not support that, I rewrote the argument to say independence between reviewers was what mattered, while keeping the same recommendation. Same conclusion, different reason. Codex returned this on the revised draft:

"The superiority thesis fails on both legs, then the synthesis swaps in independence and still arrives at the same tooling conclusion. That is a bait-and-switch unless it shows these tools beat cheaper substitutes such as fresh-session same-model review, tests, checklists, or retrieval."

Codex caught a real pattern: I had changed the reason for the recommendation without changing the recommendation. That is worth taking seriously. But catching a pattern is not the same as breaking an argument. The right response was to weigh what Codex said against other inputs, not capitulate to its verdict. Reviewer outputs are things to think about, not answers to accept.

## The workflow

Here is the routing, step by step:

| Step | Primary tool | Dimension that matters most | Fallback |
|---|---|---|---|
| Fact-check | Gemini Pro with grounded search | Tool / grounding independence | Claude sub-agent with WebSearch |
| Adversarial critique | OpenAI's Codex via the codex-rescue agent | Model independence | Claude argument-critic sub-agent |
| Steelman | Claude steelman sub-agent in fresh context | Context independence | (none) |

The asymmetry is what the three-dimension framework predicts. Cross-family tools are primary for fact-check and critique, because different grounding and different training lineage are what each of those steps specifically needs. A Claude sub-agent is primary for steelman because context independence is the only dimension that matters for defense; a fresh Claude instance that did not write the thesis has the right properties, and model independence does not add anything.

The three review steps run in strict sequence, not in parallel, and each step sees the outputs of all prior steps. Fact-check comes first so factual ground is settled before the argument is debated. The critique then focuses on reasoning structure, noting where assumptions rest on contradicted facts rather than re-arguing them. The steelman concedes contradicted facts cleanly and defends the reasoning that survives. Independence is preserved through different model family and context, not through information withholding.

The full six-step recipe:

1. Draft the claim, including evidence you intend to cite.
2. Grounded fact-check on atomic and long-passage claims.
3. Adversarial critique of the reasoning, seeing the fact-check verdicts.
4. Steelman pass, seeing both the fact-check and the critique.
5. Review the skill's summary. Drive synthesis through follow-up questions.
6. Sleep on it.

## The skill and agents

Here is the skill itself. It lives at `~/.claude/skills/truth-seeking/SKILL.md`. A Claude Code skill is a markdown file the model loads and follows as instructions.

````markdown
---
name: truth-seeking
description: Fact-check, adversarially critique, and grounded-steelman
  a thesis via Gemini, Codex, and Claude sub-agents, then offer a
  plain-language summary and open a dialogue so the human drives
  synthesis through questions.
---

# Truth-seeking cross-model review

Steps 2, 3, and 4 run in strict sequence; each step sees the outputs
of all prior steps.

## 1. Identify the target
Ask the user if unclear.

## 2. Fact-check
Invoke the gemini plugin's companion script directly via Bash. Three
output states: clean JSON, degraded (plugin salvaged raw reasoning),
or failure (fall back to the fact-checker sub-agent).

## 3. Adversarial critique
Agent tool with subagent_type: "codex:codex-rescue", passing thesis
and fact-check output. Fallback: argument-critic sub-agent.

## 4. Steelman
Agent tool with subagent_type: "steelman", passing thesis, fact-check,
and critique. No fallback.

## 5. Store outputs internally (do NOT display)
Keep FACT_CHECK, CRITIQUE, STEELMAN in context. Do not print.

## 6. Summary and dialogue
Write a plain-language summary as practitioner insight, not a debate
recap. Factual status first, then the one or two things worth thinking
about. No reviewer-name references. No verdict, ranking, or
recommendation. End with: "What do you want to look at more closely?"

On follow-up, retrieve from stored outputs and cite which reviewer
said what.
```
````

The step 6 design is the question most people will ask about. Two earlier versions were rejected. Refusing synthesis entirely and making the reader parse three thousand words alone was rigid and wasted the reader's time. Having Claude deliver a full synthesis with a verdict had the acquiescence problem; readers rubber-stamp what a capable AI already decided. The shipped version is a plain-language summary as practitioner insight, followed by the prompt "What do you want to look at more closely?" The raw reviewer outputs are stored but not shown by default; the reader retrieves specific pieces through follow-up questions.

Two of the sub-agents it dispatches are new files in `~/.claude/agents/`. The `argument-critic` agent adversarially reviews an argument, naming three to five unstated assumptions and motivated-reasoning patterns without delivering a verdict. The `steelman` agent produces the strongest defense of a thesis against a specific critique in fresh context, with explicit rules to avoid balanced synthesis. The existing `fact-checker` agent is used as well.

## Close

This post extends the custom-agents pattern from the earlier Claude Code posts on configurations and on running multiple agents in parallel. Agents can encode specific thinking roles (critic, defender, verifier) that compose across plugins into a workflow I would not bother to run manually but do run when a skill makes it cheap.

One sanity check to end on. If you ran the opposite framing right now, would your conclusion flip? If yes, you do not have a conclusion yet; you have the most recent reviewer's take. That question is probably more useful than any specific tool I have described. Ship with that.
