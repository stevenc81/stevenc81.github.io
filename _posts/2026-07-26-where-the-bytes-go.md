---
layout: post
title: "Where the bytes go"
date: 2026-07-26
---

A long AI thinking session comes to about 40 kilobytes of text, a fraction of one phone photo. The machine serving that session holds gigabytes for it, close to 80,000 times more.

Where did the bytes go?

In [my last post](/posts/how-ai-actually-thinks/) I described AI thinking as a scratchpad: the model writes out small practiced steps and re-reads them as it goes, layer two of the four-layer stack. What I skipped was what the scratchpad costs. The text is nearly free. The machinery underneath it is why long thinking is expensive to serve, and it connects to something you can feel in a store: the memory in your next laptop costs more than it did last year. Every number below is one I worked out on a napkin, and you can too.

## The machine can't afford to re-read

Every time the model adds one more token to its scratchpad, roughly a word or a chunk of one, it looks back over the whole scratchpad so far. That looking-back is what lets step forty build on step seven.

I had assumed it was looking back at the text. It is not, and that is the whole post.

Before the model can use an earlier word, it has to work out what that word meant there. Seeing the letters "bank" is not enough. A river bank and a savings bank are different things, and the model tells them apart from the words that came before. That work runs in 80 stages, each one refining the last.

Now picture throwing all of that away and starting over. To write token 5,001, the model would redo those 80 stages for all 5,000 tokens behind it. Then, to write token 5,002, it would do the whole thing again for all 5,001. For ordinary serving, that is prohibitively slow.

So the model keeps what it worked out. The first time a token goes through, the result gets filed away, and every later token reads the file instead of redoing the work. Engineers call the filed material the key-value cache. I'll call it the working copy.

Chess players know this trade. The move Nf3 is three characters, but what a strong player understands about the board after Nf3 is enormous, and nobody rebuilds that understanding from the move list every turn. You hold the position in your head. The scratchpad text is the move list. The working copy is the board.

## The napkin math

How big is one token's entry in the working copy? Meta's 70-billion-parameter Llama is publicly available with a published architecture, so you can work it out yourself.

The model has 80 layers, and each layer keeps two notes per token. One note is the token's findability tab, what the model matches against when deciding whether an old token is relevant right now. The other is the contents, what that token contributes in proportion to the match. A filing card: the tab you search by, the card you read. These are the key and the value, which is where the cache gets its name.

Each note holds 1,024 numbers, and the reason is more interesting than I first assumed. Internally the model represents a token-in-context with 8,192 numbers, spread across 64 attention heads. It does not keep 64 sets of notes. It keeps 8, each shared by a group of 8 heads, so 8 sets times 128 numbers is 1,024. That sharing is already a compression trick, [adopted to shrink the working copy and speed up inference](https://arxiv.org/pdf/2305.13245). Hold that thought for the end.

Each number takes 2 bytes. A common 32-bit floating-point number takes 4, so this is another place the engineers went hunting for savings.

So: 2 notes, 1,024 numbers each, 2 bytes per number. That comes to 4 kilobytes per layer, and across 80 layers, 320 kilobytes per token.

In ordinary English the token's text runs about 4 bytes. Its working-copy entry is 320 kilobytes. There is the 80,000x from the opening, and the gap is structural. The text is the notation. The entry is the understanding, held at every depth of the machine.

A 10,000-token think, which reasoning models produce on hard problems, comes to 3.2 gigabytes of working copy. One conversation, one user. The exact figure belongs to this model and other designs compress harder, but for large models, long thinking runs into gigabytes.

## The curve that bites

Storage is the gentle part. Every token adds the same 320 kilobytes, so 20,000 tokens hold exactly twice the cached payload of 10,000. A straight line. Nothing exponential about it, whatever you may have read.

Reading is where it turns. Count the scratchpad alone: token 1 re-reads zero earlier entries, token 2 re-reads one, token 10,000 re-reads 9,999. Add that up and the scratchpad produces about 50 million entry-readings over one think, roughly 16 terabytes streamed past the processor. Double the length and that number roughly quadruples, because it grows with the square of the length.

There is a second stream running alongside it, and it is the bigger one: the model's own weights. Seventy billion parameters at 2 bytes each is about 140 gigabytes, and the serving system streams roughly that much for every round of next-token generation. That traffic can be shared. A system advancing 50 conversations together makes one pass over the weights for all 50 next tokens. Working copies get no such discount. Apart from shared prompt prefixes, every conversation carries its own.

For this model, generating a token is mostly an exercise in moving bytes.

## Why it has to be high-bandwidth memory

During token-by-token generation, an AI chip often has arithmetic to spare. What it runs out of is bytes per second, a bottleneck [engineers call the memory wall](https://www.trendforce.com/insights/memory-wall).

The weights set the floor. Every round of generation streams about 140 gigabytes of them. Hold that against a laptop using the DDR5-5600 memory standard: its two channels have a theoretical peak of roughly 90 gigabytes per second, so even at full speed it would need more than a second just to make one pass over the model. Datacenter serving makes that pass many times a second.

So the largest datacenter accelerators use high-bandwidth memory, stacked in towers beside the processor on the same package. A single stack has a 1,024-bit data path against a laptop module's 64 bits of data. The advantage is width. More lanes open at once.

The weights are what make high-bandwidth memory necessary at that serving speed. The scratchpad is why the pressure keeps climbing, and this is the part I had backwards before I did the arithmetic. Weight traffic gets shared across every conversation in a batch, so extra users are cheap on that side. Working copies get no such discount, and they grow with every token thought. In this Llama example, a 100,000-token working copy runs to about 32 gigabytes. Fifty conversations that long would need 1.6 terabytes of memory between them, more than ten times what the model itself occupies.

Capacity then puts a ceiling on the seat count. The more memory one conversation holds, the fewer others fit beside it, so long thinkers crowd out neighbors. Serving reasoning models is as much a memory business as a compute business.

## Who pays

On hard benchmarks, reasoning-model output, including the hidden thinking, has been growing at [an estimated fivefold per year](https://www.trendforce.com/insights/ai-inference-drives-memory-demand), with some responses now running to tens of thousands of tokens. All of that lands in working copies somewhere in a datacenter. Meanwhile [Micron's entire 2026 supply of high-bandwidth memory was already spoken for](https://investors.micron.com/static-files/088991c5-a249-4f66-a0a6-258d9b66f3f9), and the stuff is expensive to make in a specific way: it takes [about three times the wafer capacity](https://www.tomshardware.com/pc-components/ram/hbm-is-eating-your-ram) that ordinary memory does for the same number of bits. Factories redirected scarce capacity toward high-bandwidth and server memory, which left less for everyone else and contributed to higher laptop memory prices.

I think that is the most concrete link between AI and everyday life I know of. It arrives on the price tag of a memory upgrade.

It is also why shrinking the working copy has become an active research target. The 8-sets-instead-of-64 trick in the napkin math above is one instance, already baked into the model I used. [DeepSeek's attention design squeezes considerably harder](https://arxiv.org/html/2405.04434v5).

## No separate warehouse

Your brain holds roughly 100 trillion synapses, with memory and computation distributed through the same living network. Signals travel constantly between neurons, so it is not a traffic-free system. What it does not have is a warehouse: no separate building where the memories are stored and from which they have to be fetched. The whole arrangement runs on about 20 watts, the power of a dim light bulb.

The memory wall, then, is not a law of thought. It is one cost of this particular machine design, the one that keeps the remembering and the arithmetic in separate buildings. The 10,000-token Llama think from earlier ships 16 terabytes of working copy across the gap between them. Your own thinking has plenty of traffic too. It stays inside the same living network, with no warehouse on the other side.
