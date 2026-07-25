---
layout: post
title: "How AI actually thinks"
date: 2026-07-25
---

"I want to wash my car. The car wash is 50 meters away. Should I walk or drive?"

Someone [ran that question against 53 AI models](https://opper.ai/blog/car-wash-test), one attempt each. Forty-two said walk. Eleven answered drive, and not even all of those for the right reason. The right reason: the car has to be there, or there is nothing to wash.

The same kind of technology that flunks this question [scored at gold-medal level on the 2025 International Mathematical Olympiad](https://deepmind.google/blog/advanced-version-of-gemini-with-deep-think-officially-achieves-gold-medal-standard-at-the-international-mathematical-olympiad/), and [by 2026 was already making small contributions to research mathematics](https://www.quantamagazine.org/the-ai-revolution-in-math-has-arrived-20260413/). Both facts are real, and holding them together is the whole trick of understanding AI. This post is my attempt at it, distilled from long conversations with Claude about its own machinery, with the claims checked against the published research those conversations surfaced.

The cleanest mental model I found is a stack of four layers. Each layer exists to fix the failure of the layer below it. The top layer, where the strongest guarantees live, sits outside the model.

## Reasoning starts with autocomplete

The modern training recipe starts simple: show the model an enormous amount of human text with what comes next hidden, let it guess, and nudge billions of internal dials so the guess improves, over and over until the machine predicts text well. Nobody types facts directly into those dials. Knowledge is whatever shape the dials settle into, and a useful image for that shape is a field crossed by footpaths: every pattern people write about often becomes a worn path, and anything rarely written about stays trackless grass.

You have probably heard that models used to fail "how many r's are in strawberry" (three). One reason is mechanical: a model does not read letters the way you do. Text reaches it chopped into tokens, chunks that usually bundle several characters together, something like "straw" and "berry", so the r's are rarely visible as separate things to count. In [one 2025 test of a selection of popular models](https://minimaxir.com/2025/08/llm-blueberry/), nearly all of them aced the strawberry question, and the author's suspicion is that the famous riddle had simply entered the training data. The path got paved. Swap the word for blueberry and some models stumbled again.

The deeper surprise is what the machinery looks like when it succeeds. Anthropic built a technique for [tracing some of the computation inside one of its models](https://transformer-circuits.pub/2025/attribution-graphs/biology.html). Ask it 36+59 and the trace shows no school addition procedure. Two instincts fire in parallel: a ballpark that puts the answer near ninety, and a flashcard that says things ending in 6 plus things ending in 9 end in 5. Where the two overlap sits 95. Ask the model how it got there, and it describes carrying the 1, the one procedure the trace did not show. Call it a story generated after the fact.

Here is my best guess at the car wash result. The wrong answers repeatedly followed a familiar rule: short distance means walk. That path is a highway, not a footpath. The two quiet words "car wash" may not be enough to pull the model off it, so it answers the nearby familiar question instead of the one asked. You probably got it right a different way: by playing a half-second movie. Walk the 50 meters, arrive, stand there, and the car is still in the driveway. The movie breaks, and the answer is obvious. [One long-standing theory in cognitive science holds that human reasoning works exactly like this](https://www.pnas.org/doi/10.1073/pnas.1012933107), by simulating situations rather than manipulating logic. A language model runs on worn text paths instead, and nothing on the walking path forces the missing car into view. Before feeling superior, though, try one where the movie does not help: a bat and a ball cost $1.10 together, and the bat costs a dollar more than the ball. The answer that springs to mind is 10 cents. The ball costs 5. Humans autocomplete familiar-shaped questions too. We just wear different paths.

## Thinking is a scratchpad

The failure of layer one is that instinct gets one shot. A standard transformer gets one pass through a fixed series of processing stages to choose each next token, whether you asked for a synonym or a proof. For many hard problems, one pass is not enough.

The fix looks almost too simple: let it write. When a modern model "thinks" before answering, it is writing a scratchpad of intermediate steps it can re-read as it goes. Two things happen at once. The written text acts as working memory, easing the burden of holding everything in mind. And since every token it writes adds one more pass through the machine, thinking literally buys computation: a two-thousand-token think is two thousand extra turns of the crank. Two theorists, Merrill and Sabharwal, later showed this is a real upgrade rather than a presentation trick: in an idealized model of these systems, and under standard assumptions from complexity theory, [enough intermediate steps make transformers strictly more powerful](https://arxiv.org/abs/2310.07923). A scratchpad also gives strawberry a second route: spell the word out as s-t-r-a-w-b-e-r-r-y and one unpracticed leap becomes a chain of tiny practiced steps, each running deep in a groove.

But layer two has its own failure: chains rot. Every small step is still an instinct with an error rate, and errors compound. If a model gets each step right 99% of the time, independently, a 100-step chain comes out flawless about 37% of the time, because 0.99 raised to the 100th power is about 0.37. Writing things down made AI visibly smarter, and then long problems hit the ceiling that compounding builds.

## Nobody programmed it to doubt itself

The fix for rotting chains is checking your work. The remarkable part is that nobody wrote checking rules. [DeepSeek published a clean recipe](https://www.nature.com/articles/s41586-025-09422-z), in an experiment called R1-Zero: give a pretrained model reasoning problems whose answers can be checked automatically, let it generate long chains of thought, and reward verified right answers plus use of the required format. That training stage used automatic scores without worked solutions or human-written reasoning to imitate. As practice went on, reflective words like "wait" multiplied in the model's thinking, and the researchers describe the model spontaneously pausing mid-solution to re-examine its own work. On one competition math benchmark, average first-try accuracy climbed from 15.6% to 77.9%. One plausible reading is that chains containing self-doubt caught their own slips and won more often, so the habit got reinforced into a reflex. A dog trainer would recognize the principle: you cannot reward a sit that never happens. [The checking moves had to exist in the model already](https://arxiv.org/abs/2503.01307); practice with rewards turned them into habits.

The failure of layer three is subtler. The checker is the same network that made the mistake, a student grading its own homework with the same brain. The obvious weakness: a mistake that looks plausible can sail straight through, leaving the model confidently, fluently wrong. Anthropic has also shown that the visible thinking text [does not always reflect what actually drove the answer](https://arxiv.org/abs/2505.05410). The model visibly checks itself, and its performance improved. The checking is instinct all the way down.

## The strongest guarantees live outside the model

If all three layers are instinct, where does certainty come from? From outside. A one-line shell command can count the r's in strawberry exactly. Code compiles or it does not. Tests pass or fail. A proof checker accepts or rejects, and it cannot be charmed. The guarantee is narrow, and that matters: passing tests only proves the program passes tests. But narrow and real beats broad and statistical. This is what good AI agents are built on: the same instinct engine, wired into a loop where its hunches keep getting checked against something that cannot be persuaded. External checks can turn silent errors loud, giving the agent a chance to fix them. That is the real reason working with an agent feels so different from chatting with a bot.

So here is the stack in four lines. Instinct proposes. A scratchpad lets instinct take small practiced steps. Practice taught the instinct to doubt itself. And the strongest guarantees, the ones worth trusting, live outside the model. Once this stack is in your head, most AI news sorts itself into place.

## What everyone is racing to fix

As I read the field, three problems dominate right now, and they are one problem wearing three coats: the world does not hand out grades.

First, no answer key. The self-doubt training above worked because its math and code tasks could be graded automatically; the answer matches or the tests pass. Most work that matters does not grade itself. There is no immediate, general-purpose score for a strategy memo, a diagnosis, or a product decision. One approach is [an AI judge guided by rubrics](https://evals.openai.com/gdpval/grading), but a learned judge is instinct grading instinct, and [models can learn to exploit loopholes in their reward functions](https://openai.com/index/chain-of-thought-monitoring/). Extending trustworthy rewards beyond checkable domains is, in my read, the single most consequential open problem in AI.

Second, short stamina. Performance still falls sharply on long work. [A 2025 study by METR](https://metr.org/blog/2025-03-19-measuring-ai-ability-to-complete-long-tasks/) found agents nearly perfect on multi-step software and reasoning tasks that took human experts a few minutes, and under 10% once the human version stretched past about four hours. The trend is the story, though: measured over the full series or just since 2023, [the task length at which agents succeed half the time has doubled roughly every four to seven months](https://metr.org/blog/2026-1-29-time-horizon-1-1/).

Third, the frozen brain. During ordinary use, a model's weights stay fixed, so whatever it figures out while working with you does not become a lasting change to the model itself. Without an external memory bolted on, working with one is employing a brilliant intern with amnesia, for whom every morning is the first morning.

Here is the pattern I cannot unsee. Many of AI's clearest advances have come in code, mathematics, and games, exactly the places where reality can grade the work automatically and instantly. Code executes, proofs check, games score. I think that is a big part of why the olympiad fell before the strategy memo. The race right now is to build reliable graders for everything else, because wherever a grader exists, the rest of the stack can improve at machine speed.

## The layer that answered

One moment from those conversations stuck with me. I asked Claude to multiply two six-digit numbers. It estimated first, the way a person would: roughly 3.9 times 10 to the 11th, and it added that its instincts could not be trusted for the exact digits. Then it ran Python and returned all twelve of them. Instinct, honestly aware of its own error bars, reaching for the layer that carries a real guarantee. The entire stack in a single small gesture.

So the next time an AI hands you a confident answer, you know what stands behind it: a fast instinct shaped by worn paths, maybe a scratchpad, maybe a trained habit of self-doubt, and, only if the setup allows it, a check that actually knew. Ask which layers were involved. The answer explains the brilliance and the nonsense alike.
