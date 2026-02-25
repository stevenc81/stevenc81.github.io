---
layout: post
title: "Why AI can explain your problem but won't lose sleep over it"
date: 2026-02-25
---

I had a thought recently: "LLMs have insights but not foresight." It felt true, but I wanted to understand *why*. So I went deep on it, and ended up touching on emotions, robot bodies, and what it actually takes to worry about tomorrow.

## The claim holds up, but needs sharper language

LLMs are good at seeing patterns. They draw connections across domains and often surface things you hadn't considered. That feels like insight, and in many practical senses it is.

What they can't do is anticipate. They won't warn you about a problem you didn't ask about. They won't lie awake worrying about a deadline. They respond to what's in front of them with impressive depth, but they never look around corners on their own.

A better way to say it: LLMs have predictive capability, but not proactive anticipation. They can calculate the trajectory of a thrown ball if you ask, but they'll never flinch before it hits them.

But this raises a question. If a senior engineer says "this architecture will cause problems at scale," isn't that also just pattern matching from past experience? What makes human foresight different from an LLM extrapolating "if X, then likely Y"?

The difference isn't in the pattern matching. It's in the biological hardware running the pattern.

## The four things you need to actually care about tomorrow

To actually care about tomorrow, you need four things working together. LLMs have none of them.

### Emotional weighting

Fear makes you flinch. Anxiety makes you plan. Regret makes you avoid repeating mistakes without being asked. These aren't decorative feelings. They're the runtime of anticipation. Neuroscientist Antonio Damasio studied patients with damage to the emotional centers of their brains but perfectly intact logic. They became incapable of planning for the future. Without emotions to weight different scenarios, every possible outcome looked equally valid. A system that doesn't feel loss has no reason to avoid it.

### Continuity of self

You plan ahead because you'll be the one dealing with consequences. An LLM has no future self to protect. It exists for the duration of a conversation and then it's gone. Even if you gave it perfect emotional simulation, without persistence across time there's nothing to be anxious *about*.

### Vulnerability

You need to have something at stake, a bounded state that's constantly under threat. This doesn't require a physical body. A digital agent with a limited compute budget it must fight to maintain would face the same pressure. The point is having a boundary condition you can't afford to cross.

### Agency

Anticipation evolved to drive action. You anticipate the ball so you can duck. You anticipate winter so you can store food. If an entity can observe and feel but never act, anticipation is wasted energy. An LLM is a brilliant consultant trapped in a soundproof room. It can write a perfect strategic memo, but it can't touch the steering wheel.

## OK, so give the AI a body?

This is the obvious next thought: put the LLM in a robot and it gets agency and vulnerability for free. Two of four pillars, just like that.

But consider the Roomba. It has a body. It has agency (movement). It has vulnerability (battery). It even has crude continuity (map memory). It still doesn't anticipate anything. It doesn't conserve battery today because it expects a bigger mess tomorrow. It reacts.

The gap between "has a body" and "has foresight" is the gap between a Roomba and a person.

What's still missing is continuity (current LLMs reset between conversations, and a robot chassis doesn't change that) and valence (a robot body generates sensor data like low battery, but that's just numbers, not feelings). For low battery to function as something like fear, it would need to change *how* the system thinks about everything, not just trigger a "go charge" rule. The difference between a thermostat and feeling cold is that feeling cold changes how you approach every other decision you're making.

## The two halves that don't fit together

There's an irony in current AI. We already have systems with a form of anticipation. They're called Reinforcement Learning (RL) agents. An RL agent playing chess or StarCraft calculates future consequences of current actions and will sacrifice short-term advantage for long-term payoff. AlphaStar repositioned its army *before* attacks happened, anticipating the opponent's build order.

And we have LLMs, which understand the world broadly but live in a frozen moment with no goals.

RL has temporal reasoning but is narrow. LLMs have broad knowledge but no temporal loop. The obvious idea: use the LLM as the world-model powering an RL agent's decisions. Combine breadth with anticipation.

This merger is where the field is headed, and it's where things get hard. The problems compound.

Start with speed. RL agents train by making thousands of decisions per second. LLMs take hundreds of milliseconds per response. It's like trying to learn tennis when your brain takes 30 seconds to process each swing.

But even if you fix the speed, the math breaks. RL learns using calculus. It needs smooth, continuous signals to slide toward the right answer. LLMs produce discrete words, and a word isn't a smooth curve, it's a hard jump. You can't pass a mathematical gradient backward through the word "apple" to tell the system which part of its reasoning went wrong.

And one ambiguous word can cascade. RL already struggles with figuring out which action 500 steps ago caused today's failure. Add an LLM reasoning in natural language, where a single vague sentence compounds errors across thousands of steps, and the signal about what went wrong gets impossibly diluted.

So you need the LLM to learn from experience. But here's the paradox: if you let its internal knowledge update continuously, it starts forgetting the broad world-knowledge that made it useful in the first place. Freeze the weights and it can't adapt. We lack an artificial equivalent of the hippocampus, the brain structure that moves short-term experience into long-term memory without overwriting what's already there.

Suppose you solve all of that. You still need to tell the system what "good" means. In games, success is clear: you win or lose. In the real world, what does "do a good job" mean? These vague goals are easy to game. An AI smart enough to find loopholes in its own reward function is an alignment problem.

And even with the right goals, there's a translation gap. LLMs think in language. The physical world runs on continuous signals: motor torques, joint angles, pixel arrays. Language is lossy. An LLM can say "gently pick up the egg," but translating "gently" into exact gripper force limits is an unsolved problem.

## The System 1 / System 2 workaround

Robotics has borrowed an idea from Daniel Kahneman's *Thinking, Fast and Slow*: give the robot two systems. System 1 is a fast RL policy handling reflexes, balance, and motor control at 500 decisions per second. System 2 is an LLM handling high-level planning and reasoning at a slower pace. This is how robots like Figure 01 work today.

It's a smart workaround. Your reflexes don't wait for your prefrontal cortex, and neither does the robot's System 1 wait for its LLM. The speed problem is largely solved. System 1 learns "gently" through trial and error with actual physical objects, so the execution side of the translation gap gets better too.

But there's a catch with planning. System 1 has physical experience. It has dropped eggs, crushed them, learned force limits. System 2 has never touched an egg. It knows "eggs are fragile" the way it knows "Paris is the capital of France," as a statistical pattern in text. A human who has held an egg once has a richer understanding of its fragility than an LLM that has read everything ever written about eggs. You felt the shell flex under your thumb. You know a toss is wrong not because someone told you, but because your body remembers how little force it took.

System 2 plans without physical intuition. System 1 executes without the ability to plan. The planner and the executor have different knowledge, and the planner doesn't know what it doesn't know.

And there's a bigger gap. Kahneman's framework models *cognition* (how we think) but not *affect* (why we think). It leaves out the limbic system, the ancient brain architecture that handles threat, reward, fear, and attachment. The limbic system isn't a third thinking system. It's the priority scheduler for the other two. It decides what's urgent, what's worth remembering, and what to ignore.

When you see a snake, your limbic system fires in about 12 milliseconds, before you consciously register what you're looking at. It floods your body with adrenaline. That chemical flood reconfigures both System 1 (jump back, sharpen reflexes) and System 2 (stop thinking about lunch, focus on escape). Without the limbic system, both systems just run, processing whatever's in front of them with equal weight. Nothing is more important than anything else.

Current humanoid robots have built the cortex: a fast reactive system and a slow deliberate one, taped together. They haven't built the limbic system. There's nothing underneath that says "protect the egg because breaking it *matters*." The robot protects the egg because it was told to. The moment instructions are ambiguous, it has no internal compass.

## Where this leaves us

We know the ingredients for foresight: emotional weighting, continuity, vulnerability, and agency. We know the pieces exist in separate fields. RL has temporal reasoning, LLMs have broad knowledge. We have a reasonable architectural sketch in the System 1/System 2 approach.

What we don't have is the recipe for combining them. The merger is blocked by real engineering problems (speed, gradients, forgetting) and deeper conceptual ones (what is reward in the real world? how do you build something that cares?).

If someone eventually builds a persistent system with functional emotional weighting, continuous learning, vulnerability, and agency, the distinction between "insight" and "foresight" would probably collapse. But at that point you're no longer building an LLM. You're building something new.

For now, LLMs remain remarkably good at seeing what's in front of them. They just don't look up.
