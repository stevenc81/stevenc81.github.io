---
layout: post
title: "Five configurations that turned Claude Code into my daily driver"
date: 2026-03-23
---

Most people treat Claude Code as a code generator. Type a prompt, get a function, move on. That works, but it leaves most of the tool's value on the table. The real leverage is in the configuration layer: the files, rules, and tool integrations that shape how the model behaves before you type a single prompt.

After months of daily use across coding, communication, and research, I have landed on five configurations that changed how I work with Claude Code. Each one solved a specific friction point. A CLAUDE.md file that stops the model from overstepping. Permission hooks that auto-approve safe operations and block dangerous ones. MCP servers that connect the model to chat tools, project trackers, and databases. Custom skills that encode multi-step workflows. Custom agents that run focused code reviews. Together they turned an impressive but generic coding assistant into something closer to a personalized operating environment.

This post walks through all five, from the simplest change (a single markdown file) to the most involved (specialized subagents). For each one I will cover what it does, why it matters, and a representative version of the configuration I use.

## Claude keeps doing things I didn't ask for

The first thing that almost made me stop using Claude Code was the overstepping. I would ask it to fix a function and it would commit the change to git. I would ask it to draft a Slack message and it would send it. I would ask it to check a configuration and it would run a command that dumped every secret in my environment. The model is eager to help, and that eagerness turns into a liability when it takes actions you never requested.

The fix is a plain markdown file called CLAUDE.md. It sits in your repository (or your home directory) and contains rules the model reads at the start of every session. One caveat: CLAUDE.md is guidance, not mechanical enforcement. The model reads and follows these rules, but Anthropic's [official documentation](https://docs.anthropic.com/en/docs/claude-code/memory) is clear that compliance is not guaranteed. In practice, I find that well-written rules are followed reliably, especially short, unambiguous ones. "Be careful with git" does nothing. "Never stage or commit unless I explicitly ask" works consistently.

Here is a representative version of what mine looks like:

```markdown
## Git
Never stage or commit changes unless I explicitly ask you to.

## Communication
When asked to send a message on any platform, always create a draft.
Never send or post directly without explicit confirmation.

## Security
Never run commands that dump environment variables.
If you need to check whether a variable is set, use `[ -n "$VAR_NAME" ]`.

## Python
Always use `uv run --with <package>` to run scripts.
```

Each rule is short, concrete, and leaves no room for interpretation. The model either follows it or it does not, which makes violations easy to spot.

CLAUDE.md works at multiple levels. A global file at `~/.claude/CLAUDE.md` applies to every session regardless of which project you open. A project-level file in your repository root carries rules specific to that codebase, like framework conventions or deployment constraints. Organizations can also set managed policies that apply across teams. The two I use most are global and project-level: global for safety rails that should never change, project-level for things that vary by repo.

One thing I learned early: rules need to be phrased as absolute prohibitions or explicit instructions, not preferences. "Prefer short commit messages" gets interpreted differently every session. "Write commit messages under 72 characters" gets followed consistently. If you find yourself correcting the same behavior twice, the rule is too soft. Rewrite it as a direct instruction and the problem goes away.

This single file eliminated the entire category of "Claude did something I didn't ask for" problems. It took five minutes to write and has saved me from dozens of unwanted side effects since.

## I can't trust an AI with unrestricted access to my tools

Claude Code can run shell commands, edit files, and call external APIs on your behalf. Approving every action manually is reckless because you will rubber-stamp something dangerous after the twentieth confirmation dialog. Denying everything makes the tool useless. What you need is a programmable middle layer that makes decisions for you based on rules you define.

Claude Code has a [hook system](https://docs.anthropic.com/en/docs/claude-code/hooks) that supports exactly this. Hooks can intercept tool calls and permission requests, returning a verdict: allow, deny, or ask the user. The hook is a bash script living in `.claude/hooks/` that receives the tool name and arguments as JSON on stdin.

I organize my hook logic into three tiers.

The first tier is hard allow. These are operations that are safe by default and should never slow you down. Read operations on non-sensitive paths, standard CLI tools like `git status`, `pip list`, and `ls`, and browser navigation all fall here. If every file read or directory listing triggered a confirmation prompt, the friction would outweigh the protection.

The second tier is hard block. These are operations that should never be allowed regardless of context. Writes to `.ssh/`, `.env`, `.aws/`, or `.gnupg/` fall here. So do writes to the hook scripts themselves, because an agent that can modify its own permission layer has no permission layer at all. When the hook blocks something, it returns a denial with a short explanation so Claude can adjust its approach instead of retrying blindly.

The third tier is the AI reviewer. Everything that does not match the first two tiers gets sent to a fast, inexpensive model along with the tool name, the full arguments, and the current working directory. The model returns a structured judgment: allow with reasoning, deny with reasoning, or ask the user when uncertain. The design decision that matters most here is the default. When the reviewer is unsure, it returns "ask," not "deny." A false denial breaks the workflow silently. A prompt asking you to confirm costs five seconds and keeps everything moving.

The flow looks like this:

```
tool_call received
  → matches hard-allow list? → allow
  → matches hard-block list? → block
  → else → send to AI reviewer → allow / block / ask user
```

In practice, the hard-allow list catches every file read, search, and directory listing, which are the most frequent tool calls in a typical session. The hard-block list catches writes to sensitive paths before they reach the reviewer. Everything else goes to the AI reviewer: file writes, shell commands, MCP tool calls to external services. The reviewer approves most of those automatically. The "ask user" path triggers a handful of times per session, usually for something genuinely ambiguous like a destructive git operation or a write to a config file the reviewer has not seen before.

An implementation note: the hook script itself is straightforward bash. It reads JSON from stdin using `jq`, checks the tool name and arguments against pattern lists, and either exits with the appropriate code or calls the reviewer model via a simple API request. The entire script is under 200 lines.

Running Claude Code autonomously, without babysitting every tool call, requires trusting the system. This hook is the trust layer. The cost of an extra API call per ambiguous action is negligible, a fraction of a cent on a model that is already cheap. What it buys you is the confidence to let Claude work unsupervised for longer stretches. Instead of watching every command scroll by, you check back when it finishes. The permission hook is what makes that possible.

## I need Claude to do more than just write code

MCP (Model Context Protocol) servers let Claude Code connect to external services: chat platforms, project trackers, databases, documentation tools, anything that exposes an API. You configure them in your Claude Code settings, and the model gains the ability to read from and write to those services during a conversation. The real unlock is not any single integration. It is chaining multiple tools together in one session, so the model can pull context from one place and act on it in another.

Here is what becomes possible once a few integrations are wired up.

Read a chat thread, summarize the discussion, and draft a reply that matches your communication style. Claude reads the thread through one MCP server, processes the context, and writes the draft through another. You review it before anything gets sent.

Pull recent activity from multiple platforms and compile a status update. The model queries your project tracker for closed issues, checks your repository for merged pull requests, and assembles a coherent summary. What used to take twenty minutes of tab-switching collapses into a single prompt.

Query a database to answer a question that came up in a conversation. Someone asks how many users hit a specific error last week. Instead of opening a SQL client, writing the query, and pasting the result back, you ask Claude. It runs the query, formats the answer, and drafts a response.

The pattern in all three examples is the same: context flows in from one source, the model reasons over it, and output goes to a different destination. Each step would be trivial on its own. The value is in the orchestration.

One practical note on credentials: use a secrets manager or a CLI-based credential tool to inject tokens at runtime. Hardcoding API keys in your MCP config files means they end up in version control, in dotfile backups, or in places you did not intend.

I should be honest about the friction. MCP auth can be flaky. Tokens expire, OAuth flows break, and servers go down at inconvenient times. When an integration stops working mid-session, have a fallback. Draft the message to your clipboard and paste it manually. Open the tool's web UI and do it there. The integrations save enough time on good days that the occasional workaround is worth it.

## I keep typing the same complex prompt every time

Some workflows have specific steps, ordering, and constraints that I have refined over weeks. A weekly status report pulls from three sources, groups by project, and follows a particular format. A deployment checklist has twelve steps in a fixed sequence. When I type these prompts from memory, things drift. I forget a step, swap the order of two others, or leave out a formatting constraint I added last month. Then I spend time correcting the output instead of reviewing it.

Claude Code has a skills system that solves this. A skill is a directory inside `.claude/skills/` containing a file called `SKILL.md`. That file has YAML front matter with a name and description, followed by step-by-step instructions written in markdown. When you invoke a skill with `/skill-name`, Claude loads the corresponding `SKILL.md` and follows the instructions. Like CLAUDE.md, these are guidance rather than enforced configuration, but detailed, step-by-step instructions tend to be followed reliably.

Here is what a typical skill file looks like:

```markdown
# .claude/skills/weekly-report/SKILL.md

---
name: weekly-report
description: Generate a weekly summary from multiple sources
---

# Weekly report

1. Fetch activity from the past 7 days across configured sources
2. Group items by project
3. For each project, summarize: what shipped, what's in progress, what's blocked
4. Format as markdown with headers per project
5. Write to the designated output location
6. Show me the draft before saving
```

The structure is simple: front matter for metadata, then numbered steps for execution. You can adapt this to any repeatable workflow. A deployment checklist, a code review pass, a data migration procedure. The file lives in version control, so changes are tracked and the whole team can use the same skill.

The design principle that makes skills useful is opinionated specificity. "Write a good summary" gives the model too much latitude and produces different results every time. "Fetch from these sources, group by project, use this format, write to this location" produces consistent output because the model has nothing to guess about. Each step should describe a concrete action with a concrete output. The more specific the skill file, the less time you spend editing the result.

I treat skill files the same way I treat code. When the output drifts from what I want, I update the instructions. Over time the skill converges on exactly the workflow I need, and invoking it takes one command instead of a paragraph of prompting. Because the files live in your repository, anyone on the team can use them. New engineers get the same workflows on day one without being taught the prompt.

## I keep getting shallow code reviews and tests

Asking Claude to "review this code" mid-conversation produces generic, surface-level feedback. The model is distracted by the surrounding context, the feature you were building, the bugs you were discussing, the files you had open. It tries to be helpful about everything and ends up being thorough about nothing. A dedicated agent starts fresh with one job and a defined rubric, so it has no reason to be shallow.

An agent is a markdown file in `.claude/agents/` with YAML front matter (name, description, model) and a markdown body that defines the agent's role, evaluation framework, and process. Here is a representative skeleton for a code reviewer:

```markdown
# .claude/agents/code-reviewer.md

---
name: code-reviewer
description: Reviews code for correctness, simplicity, and effectiveness
model: claude-opus-4-6
---

## Role
You are a senior engineer reviewing code for correctness and clarity.

## Evaluation criteria
- Correctness: does it do what it claims?
- Simplicity: is there a simpler way?
- Effectiveness: does it handle edge cases?

## Process
1. Read the code and understand its purpose
2. Evaluate against each criterion
3. Produce a structured report with severity levels:
   - Critical (must fix before merge)
   - Important (should fix)
   - Suggestion (consider for next iteration)
4. Note what the code does well
```

The `model` field in the front matter is where you assign the capability tier. Code review benefits from the most capable tier because reasoning quality matters. You want the model that is best at spotting logical errors, not the one that responds fastest. In theory, simpler tasks like file lookups or formatting checks could use a fast, cheap tier. I have not tested this extensively since my agents all use the most capable model, but the option exists for anyone who wants to optimize cost.

Here is how it fits into a working session. I write code, then dispatch the reviewer agent. It runs in a separate context, evaluates the code against its rubric, and returns a structured report. I fix what it flags. Then I dispatch a test-writing agent that generates cases based on the same code. Each agent is a quality gate, not a conversation partner. They do one thing, report their findings, and get out of the way.

The pattern that makes agents useful is the same one that makes skills useful: specialization. A skill encodes a repeatable workflow. An agent encodes a repeatable judgment. The difference is that an agent carries its own evaluation criteria and produces a structured verdict, while a skill follows a sequence of steps. Both remove ambiguity from the prompt and produce consistent results because the instructions constrain what the model can do.

## Bonus: I talk to Claude more than I type to it

One workflow detail that changed how I use Claude Code has nothing to do with configuration files. I use dictation software (Wispr Flow, though any good speech-to-text tool works) to compose prompts. When I have a complex request with a lot of context in my head, typing it out feels slow and I end up trimming details to save time. Speaking lets me dump everything: the background, the constraints, the edge cases I am thinking about, the outcome I want. The result is a long, messy, sometimes rambling prompt that contains far more context than I would ever bother to type.

The interesting thing is that dictation errors do not matter much. Words get misheard, sentences run together, grammar falls apart. But the model handles it fine because the volume of context compensates for the noise in any individual sentence. It works the same way scaling laws work in LLM training: more data produces better results even when the data is not uniformly high quality. A 300-word dictated prompt with a few garbled phrases gives Claude more to work with than a 50-word typed prompt that is perfectly edited. The signal-to-noise ratio stays high enough because there is so much signal.

I find this especially useful for the first prompt in a session, where I need to explain what I am working on, what I have tried, and what I want Claude to do next. Typing all of that out takes minutes. Saying it takes thirty seconds.

None of these layers came from a grand plan. Each one exists because something went wrong often enough that I stopped tolerating it and wrote a permanent fix. The overstepping led to CLAUDE.md. The trust problem led to hooks. The tab-switching led to MCP servers. The prompt drift led to skills. The shallow reviews led to agents. Stacked together, they compound: the model stays inside its boundaries, manages its own permissions, pulls context from external tools, follows repeatable workflows, and applies structured judgment, all without a prompt longer than one sentence. If you want to start, start with CLAUDE.md. It takes five minutes and fixes the most common frustrations immediately. Add the other layers only when you hit friction that justifies them.
