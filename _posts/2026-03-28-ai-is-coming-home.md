---
layout: post
title: "AI is coming home"
date: 2026-03-28
---

A large AI model is running on a laptop. It generates text faster than most people read. It writes code, reasons through math problems, and holds nuanced conversations. None of the data leaves the machine. No subscription. No per-use bill. No usage limits.

This is not a research demo. This is a MacBook Pro running an open-source model through [Ollama](https://ollama.com/), a free tool that now sees 52 million monthly downloads. Three years ago, that number was 100,000.

I have been researching this trajectory, stress-testing it against the strongest counterarguments I could find. The evidence points in a clear direction: the AI that you interact with, the part that reads your questions and generates answers, is moving from remote data centers to your own computer. The training part (how models learn in the first place) still requires massive infrastructure and billions of dollars. But the part you use day to day is increasingly something your own hardware can handle.

The reasons look familiar. They are the same ones that moved computing from mainframes to personal computers: accessibility, ownership, privacy, and the peace of mind that comes from not watching a meter run.

## What is possible today

Apple's latest high-end laptop chip, the M5 Max (shipping March 2026), can move data between its processor and memory fast enough to run a 70-billion-parameter AI model at conversational speed. "70 billion parameters" means roughly the amount of learned knowledge baked into the model. For comparison, the original ChatGPT (GPT-3.5) had 175 billion parameters but required a room full of servers to run. Thanks to compression techniques that shrink models to about a quarter of their original size while keeping roughly 95% of their quality, that 70-billion-parameter model now fits in about 43 gigabytes and runs on a laptop with 128GB of memory. It generates 22-32 words per second, with over 75GB of memory left for your operating system, browser, and other apps.

Every major chip maker is moving in this direction. Qualcomm, Intel, and AMD have all added dedicated AI processing units to their latest laptop chips, with each generation roughly doubling in AI performance. NVIDIA's latest consumer graphics card can move data at nearly twice the speed of the previous generation. These are not incremental spec bumps. They are deliberate design choices to make your computer better at running AI.

The software side has kept pace. Ollama turns running a local model into a single command. Apple built its own AI framework (MLX) specifically for its chips. [HuggingFace](https://huggingface.co/), the largest open-source AI platform, hosts over 2 million models, with over 150,000 packaged specifically for local use, up from just 200 three years ago.

And it is not just text. Image generation tools running locally now produce results that compete with cloud services like Midjourney. Speech recognition running on Apple's chips matches or beats cloud transcription services in both speed and accuracy. A text-to-speech model with just 82 million parameters (tiny by modern standards) reached number one on a public quality leaderboard, beating models 15 times its size. Real-time object detection runs at hundreds of frames per second on consumer hardware. On several quality benchmarks, local AI matches or exceeds what the cloud offers.

## Why it is happening

Three forces are converging.

**Chip makers are building AI into their hardware on purpose.** Apple added dedicated AI accelerators inside every graphics core in its latest chips. Qualcomm's entire strategy for Windows laptops depends on AI processing performance. Intel nearly quadrupled its AI processing power in a single generation after Microsoft required a minimum threshold for its "Copilot+ PC" certification. AMD published academic research on its AI chip architecture. Even the organization that sets memory standards (JEDEC) titled its latest specification "Enhance Mobile and AI Memory Performance," designing the next generation of laptop memory specifically with AI workloads in mind.

These are not marketing decisions. They are architectural choices etched into silicon, backed by years of R&D, and they signal where the entire industry is heading.

**Compression techniques are making big models fit on small devices.** Three approaches are stacking. First, quantization shrinks model files to about a quarter of their original size while preserving roughly 95% of quality. Second, knowledge distillation lets researchers train small models to mimic larger ones: a model with 4 billion parameters now rivals the performance of one with 72 billion, an 18x size reduction. Third, Google published [TurboQuant](https://research.google/blog/turboquant-redefining-ai-efficiency-with-extreme-compression/) on March 25, 2026, which compresses a model's working memory during conversation by 6x with zero quality loss.

Stack these together and you get results like this: a 32-billion-parameter model that [outperforms OpenAI's o1-mini](https://github.com/deepseek-ai/DeepSeek-R1) on math competition benchmarks fits in about 20 gigabytes. That runs on a standard MacBook Pro. What required a data center two years ago runs on a laptop today.

**Companies are releasing their AI models for anyone to use.** Meta, Google, Mistral, DeepSeek, Alibaba, and as of August 2025, OpenAI itself, all publish model weights (the learned knowledge inside an AI) that anyone can download and run locally. The strategic motivations differ. Meta wants to turn AI models into a commodity so competitors cannot charge for them. Mistral builds a business around open models with premium services on top. DeepSeek demonstrates that frontier-quality AI can be built at a fraction of Western training costs. But the result is the same: capable AI models are freely available for anyone with the hardware to run them.

The cost of training a model equivalent to GPT-4 has fallen to $5-10 million. The cost of matching yesterday's best keeps dropping, even as pushing tomorrow's frontier gets more expensive. This means the supply of capable, free models will keep growing.

## The gap between local and cloud, and why it matters less than you think

Local models are not as capable as the best cloud models. That is true, and it matters less with each passing quarter.

On the standard knowledge benchmark (MMLU), the gap between the best open and closed models went from 17.5 percentage points in late 2023 to effectively zero by early 2026. On real-world coding tasks, an open model called MiniMax M2.5 scores 80.2%, placing in the top five alongside Claude and Gemini. On a math benchmark (MATH-500), a freely available model hits 97.3%.

[Epoch AI](https://epoch.ai/data-insights/consumer-gpu-model-gap), a research organization that tracks AI progress, measured the lag between what you can run on your own computer and what sits at the frontier in cloud data centers. The answer: 6-12 months, depending on the task. That gap has stayed roughly constant even as the frontier advances. Today's $2,000 graphics card runs models matching last year's best cloud AI. This is perhaps the clearest way to understand the trajectory: the frontier keeps moving, and consumer hardware follows about a year behind.

Where do cloud models still lead? Processing very long documents (cloud models can handle the equivalent of several novels at once, while local models struggle beyond a long report), running autonomous AI agents for hours, and the hardest reasoning problems. These are real advantages for specific use cases.

But the relevant question is not whether local can match the cloud on every task. It is whether local can handle what you actually do day to day. Code completion, summarization, translation, answering questions about documents, generating images, transcribing audio: for these tasks, local models are already there. The frontier gap matters for the hardest 10-15% of tasks. For the rest, local is good enough, and it comes with benefits the cloud cannot offer: your data stays on your machine, it works without internet, there are no usage limits, and there is no monthly bill that scales with how much you use it.

## The mainframe pattern

IBM dominated the computer market in the 1960s and 70s with massive shared machines that companies rented time on. The first personal computers arrived in 1977. By 1984, desktop computer sales exceeded mainframe sales for the first time, just seven years later.

The parallels to AI are real. Centralized, expensive, gatekept technology becoming personal, accessible, owned. Costs dropping until individual ownership makes economic sense. Software ecosystems springing up once the hardware is capable enough. Incumbents underestimating the demand for personal use. Ken Olsen, the founder of Digital Equipment Corporation, said in 1977 that he saw "no reason for any individual to have a computer in his home" (he was probably talking about a different concept, but the quote became a symbol of how industry leaders can miss what individuals want). Today, the implicit message from cloud AI providers carries a similar energy: you don't need your own model.

The analogy has limits. Cloud AI benefits from network effects that mainframes never had: ChatGPT's 900 million weekly active users generate feedback that improves the next model. Local models do not get smarter over time the way cloud services do. Training AI has no personal computing equivalent. And computing already re-centralized once, from personal computers back to the cloud (think of how most software moved from installed programs to web apps). History does not guarantee that the same decentralization pattern will repeat.

But here is what the history gets right: the mainframe did not die. IBM's mainframe business reached 20-year revenue highs recently. 44 of the world's 50 largest banks still run on mainframes. The personal computer did not replace the mainframe. It created a larger market alongside it. Cloud AI is not going to disappear. Local AI is creating a new, larger tier of usage alongside it.

## What this looks like in practice

The future is tiered. Your hardware determines what you can run, and different devices serve different purposes.

A high-end laptop with 128GB of memory runs 70-billion-parameter models comfortably at conversational speed, alongside a full development environment. This is a serious AI workstation: near-frontier reasoning, image generation, speech recognition, and code assistance, all local, all private.

A mid-range laptop with 48GB handles 32-billion-parameter models. These are models that beat some of OpenAI's offerings on math and reasoning benchmarks. For most daily work, coding, writing, analysis, this is more than enough.

A budget laptop or desktop with 24GB runs smaller models (8-14 billion parameters). Still capable for code completion, summarization, translation, and quick questions.

A phone or tablet runs the smallest models for lightweight tasks: dictation, quick answers, on-device translation.

Each device gets AI matched to what it can handle. The larger machines take on harder problems. The smaller ones handle routine tasks. All of them keep your data on your device.

One concern worth addressing: agents, the AI systems that browse the web, call APIs, and take actions across services, need internet connectivity. Does that make local AI irrelevant for the agentic future? Not quite. The reasoning engine, the part that processes your data and makes decisions, can still run on your machine. Only the specific actions (a web search, an API call) go over the network. When a local AI agent checks the weather, the weather service sees a city name. It does not see your code, your conversation, or your files. This distinction matters, and it works today: Ollama already supports this pattern, and Stanford's [OpenJarvis](https://scalingintelligence.stanford.edu/blogs/openjarvis/) framework found that local models handle 88.7% of queries accurately at interactive speed.

## What could change this trajectory

Three uncertainties are worth watching.

**The open model supply could tighten.** Meta has been the largest contributor of free, downloadable AI models in the West. But in July 2025, Zuckerberg wrote that superintelligence "will raise novel safety concerns" and Meta would need to be "careful about what we choose to open-source." By December 2025, Meta was developing a closed model internally. Its chief AI scientist, Yann LeCun, a vocal advocate for open AI, reportedly left the company. If Meta pulls back, Chinese labs (Alibaba, DeepSeek) would partially fill the gap, but with geopolitical complications.

**Cloud AI keeps getting cheaper.** Cloud prices for equivalent AI capability are falling at roughly 50x per year, far faster than consumer hardware improves. For light users, cloud will stay cheaper than owning hardware. The economic case for local AI holds only for people who use it moderately to heavily, roughly equivalent to spending $100-150 per month on cloud AI services.

**The hardest tasks may stay cloud-only.** Processing book-length documents, running AI agents autonomously for hours, and solving the hardest reasoning problems require infrastructure that personal computers cannot match. If these capabilities become standard expectations rather than edge cases, the case for local weakens.

## Where this leaves us

The evidence points in one direction. AI you can run yourself is coming to your devices. The hardware is being designed for it. The compression techniques that make big models fit on small machines are ahead of schedule. The models are freely available. The software is mature enough for daily use. The economics work for anyone who uses AI regularly.

The most likely future is not one where cloud AI disappears, but one where local becomes the default for most of what you do with AI. You run your models for everyday work. You reach for the cloud when you need the absolute best or when a task exceeds your hardware. This is already how Apple designed its AI system: process everything on-device by default, use the cloud only as a fallback for harder requests.

The personal computer did not kill the mainframe. It created a vastly larger market alongside it, one where individuals had direct access to computing power that previously required institutional permission. Local AI is following the same path. The intelligence that used to require a connection to a distant data center is coming to your machine.

A few things will signal whether this is accelerating or stalling:

- The lag between what runs on consumer hardware and the frontier: still 6-12 months? Narrowing?
- Meta's next major model release: will they share it openly, or keep it closed?
- Next-generation laptop memory: does it arrive on schedule in late 2026, bringing another major speed increase?
- Whether Apple, NVIDIA, or someone else ships an integrated local AI experience that makes all of this accessible to people who are not developers

Watch those. They will tell you where this is going before the conventional wisdom catches up.
