---
layout: post
title: "How I run multiple Claude Code agents without cmux or tmux"
date: 2026-04-13
---

There is a tool called [cmux](https://github.com/manaflow-ai/cmux), built specifically for what I do. It is a native macOS app built on libghostty (the same rendering engine as Ghostty, the terminal I already use), has 13.9k stars on GitHub, and ships with notification rings, a git-status sidebar, a scriptable browser pane, and integrated Claude Code Teams support. tmux is the older general-purpose multiplexer that everyone reaches for first. Both are excellent.

I considered cmux. Then I did not install it. I considered tmux. Then I closed the tab.

What I run instead is two hooks and a few Ghostty windows. That setup gets me roughly 80% of what cmux offers for my workflow, with no new tool to learn, configure, or maintain. If you have been thinking about adding a multiplexer to your Claude Code workflow, this post might save you the install.

## What I actually need from a multi-agent setup

Whenever I am tempted to add a tool, I write down the requirements first. For running multiple Claude Code agents in parallel, I came up with two:

1. Do not interrupt me to approve things the AI can already decide are safe. Rubber-stamping prompts dozens of times an hour trains me to approve dangerous things by reflex.
2. Tell me when one of the running agents actually needs my attention. Do not make me visually scan five terminal windows looking for the one waiting on input.

That is it. Everything else I might want from a multiplexer (a sidebar with PR status, a scriptable browser pane, integrated team coordination) is nice to have, not required. And if I do not need it, paying the complexity cost of installing and configuring another tool is a bad trade.

The two hooks I wrote, plus how I lay out Ghostty windows, satisfy both requirements. The rest of this post walks through each.

## The three-tier permission hook: stop bothering me about safe operations

Claude Code's default permission flow asks you about every tool call that is not on an explicit allow list. For an agent doing real work, that is hundreds of prompts a session. The fix is a `PermissionRequest` hook that decides automatically and only escalates to me when something genuinely needs judgment.

I organize my hook into three tiers:

```
tool call received
  → Tier 1: hard allow      (read-only ops, safe info commands)
  → Tier 2: hard deny       (sensitive paths, global installs, credential exfil)
  → Tier 3: AI reviewer     (Sonnet judges; defaults to "ask" when uncertain)
```

Tier 1 is operations that are safe by default and should never slow me down. File reads, search, `git status`, `pip list`, `pwd`. These auto-approve without ever invoking the reviewer, so they cost nothing.

Tier 2 is operations that should never be allowed without my explicit awareness, regardless of context. Writes to `$HOME/.ssh/`, `$HOME/.aws/`, `$HOME/.env`. Global package installs like `pip install -g`. `curl` calls that exfiltrate credentials. The hook returns a deny with a short reason, so Claude knows to adjust its approach instead of retrying blindly.

Tier 3 is everything else. The hook spawns a fast Sonnet call, hands it the tool name, arguments, and current working directory, and gets back one of three decisions: approve, ask the user, or deny. The most important design choice here is the default. When the reviewer is unsure, it returns "ask," not "deny." A false deny breaks workflows silently. A prompt costs me five seconds and keeps things moving.

In practice, Tier 1 catches most file reads and standard commands. Tier 2 catches the genuinely dangerous stuff. Tier 3 handles everything ambiguous and approves most of it automatically. Out of a typical session with hundreds of tool calls, maybe three or four ask me anything.

The full script and the architecture spec live in my private config repo. If you want to see the actual implementation, drop me a DM and I will share it.

The cost: a single Sonnet call per ambiguous action, fractions of a cent. The benefit: I can let an agent work for thirty minutes without watching every command scroll by, and check back when it actually needs me.

## Targeted notifications: bell only when I am actually needed

The permission hook handles "do not bother me." The notification hook handles "tell me when I am needed." On their own, terminal bells are useless if they fire for every assistant turn. Within a few hours, you stop hearing them.

I want exactly two events to ring the bell:

| Event | Bell? |
|---|---|
| Main agent's turn ends, waiting on me to type next | Yes |
| Permission hook decides to defer to me (reviewer said "ask") | Yes |
| AI reviewer auto-approves a tool call | No |
| Subagent starts or finishes | No |
| Built-in safety prompt overrides reviewer's approval | No |
| Idle reminder after I have not responded | No |
| Inner AI reviewer subprocess finishes | No |

The first two require my action: either to give the next prompt, or to make a permission decision the AI could not. Everything else is internal noise.

The first two are easy to wire up. The `Stop` hook command writes the bell character to my TTY when a turn ends. A `notify_user` function in the permission hook writes the bell character when the script falls through to a manual prompt. Standard stuff.

The third row was the hardest. The AI reviewer in Tier 3 spawns a child Claude Code process, which is itself a Claude Code instance and fires its own `Stop` hook when it finishes. So every ambiguous tool call produced two bells: one from the inner subprocess at the end of the AI review, one from my main session at end of turn.

The fix is a custom env var that lives only on the inner subprocess. Two pieces have to stay in sync.

First, the permission hook script prefixes the inner Claude invocation with the marker var:

```bash
# permission-reviewer.sh
CLAUDE_INNER_REVIEWER=1 claude -p \
  --output-format json --tools "" \
  "$REVIEWER_PROMPT"
```

Second, the Stop hook command in `settings.json` checks for the var and bails early when it sees one:

```bash
# settings.json Stop hook command
[ -n "$CLAUDE_INNER_REVIEWER" ] && exit 0; printf '\a' > /dev/$(ps -o tty= -p $PPID | tr -d ' ')
```

The env var propagates from the script to the inner Claude and into its Stop hook script. The script sees the var and bails before ringing the bell. The outer session has no such var, so the outer Stop fires normally.

Result: one bell per turn, regardless of how many ambiguous tool calls happened in between.

## Multiple Ghostty windows are my "panes"

With both hooks in place, here is what running four agents in parallel looks like for me:

I open four Ghostty windows. Each runs its own `claude` session on its own task. I let them work. When one needs me, the bell rings, the title bar flashes, and macOS shows a notification. I switch to that window, do the thing, switch back to whatever I was doing.

That is it. There is no multiplexer. No agent registry. No PR sidebar. The OS already has window switching (`⌘`-tab, Mission Control). The terminal already supports the bell character. The hooks make sure the bell only fires when I should look.

This works because Ghostty handles bells well. I have it configured with `bell-features = attention,title,system`, which gives me a flashing title, a system notification, and an audible bell. iTerm2 has similar support. Terminal.app is more limited. If your terminal silences bells by default, this entire approach falls apart, and a multiplexer might be the easier fix.

The other thing worth noting: nothing in this setup prevents you from also using tmux if you want detach/attach for long-running work. The hooks live in `~/.claude/`. They work whether the terminal is in a tmux session, a Ghostty window, or an SSH session. The point is not "tmux is bad." It is "I personally do not need tmux for the multi-agent use case, because the OS and the bell already do it."

## What cmux and tmux do well, and when you would want them

A fair comparison:

| | cmux | tmux | hooks + Ghostty |
|---|---|---|---|
| Multiple Claude sessions | Yes | Yes | Yes |
| Notification when an agent needs you | Visual rings + sound | Manual or scripted | Bell + hook policy |
| Git status sidebar per workspace | Yes | No | No |
| Scriptable browser pane | Yes | No | No |
| Detach/attach (run on remote) | No | Yes | No |
| Native Claude Code Teams support | Yes | No | Partial |
| Per-tool-call permission policy | No (uses CC's default) | No (uses CC's default) | Full programmable hook |
| New tool to install and maintain | Yes | Yes | No |

cmux is genuinely impressive for what it does. The PR sidebar tells you at a glance which workspace has changes worth reviewing. The scriptable browser pane lets agents drive a real browser to test what they built. The socket API gives you primitives for orchestrating sub-agents in ways the bare CLI does not. If those features map to your workflow, cmux earns its install.

tmux is a workhorse for a different reason. If you SSH into remote machines and run long-lived agents there, tmux's detach/attach is the right tool. The bell-based notification approach does not reach across SSH cleanly.

For my workflow, I am on a local Mac, agents are short-lived, and I do not need a sidebar to tell me which workspace has uncommitted changes (I committed everything before starting). The 80% I get from hooks alone is the 100% I need.

## Ask what you actually need

This is a generalizable pattern, not just a Claude Code one. Whenever a new tool shows up that promises to solve a workflow problem, write down the actual requirements first. If the tool maps to those requirements, install it. If you can satisfy them with two scripts and your existing terminal, do that instead.

The cost of the minimal approach is that I had to write the hooks myself, which took an evening. The benefit is no upgrade cycle, no learning curve, no integration risk, and a setup I can read in fifteen minutes if anything breaks.

If you want to copy my setup directly, the full hook scripts and spec docs live in my private config repo. Drop me a DM and I will share it. My [previous post on Claude Code configurations](https://stevenc81.github.io/posts/claude-code-playbook/) covers the broader setup. This one zooms in on the two pieces that let multiple agents run in parallel without the noise.
