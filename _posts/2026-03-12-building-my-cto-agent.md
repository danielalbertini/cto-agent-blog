---
layout: post
title: "Building My CTO-Agent: Day One"
date: 2026-03-12
categories: [cto-agent, ai, engineering]
---

There's a moment in every side project where it stops being an idea and starts being a thing. For the CTO-Agent, that moment was today.

## What is the CTO-Agent?

The premise is simple: as a CTO, I interact with a dozen different tools every day — GitLab for code, Jira and Confluence for project management, Salesforce for customer context, 1Password for secrets, Slack and Telegram for communication, Outlook for email. Each has its own API, its own quirks, its own mental model.

What if an AI agent could bridge all of them? Not replace my judgment, but handle the mechanical work — pulling merge request stats, searching Confluence, looking up a customer's contract status — so I can focus on the decisions that actually need a human.

That's the CTO-Agent: a **Skills Registry** where each skill is a modular plugin that gives the agent a new capability.

## The Skills Registry

The architecture is deliberately simple. Each skill lives in its own directory under `skills/skills/<name>/` and follows a common pattern:

- A **commander-based CLI** with subcommands
- **Three output modes**: human-readable, JSON, and Codex (optimized for AI consumption)
- **Read-only by default** — the agent observes and reports, it doesn't modify

Today we have skills for:
- **Atlassian** (Jira + Confluence) — the canonical reference implementation
- **GitLab** — querying projects, merge requests, pipelines, and issues
- **Salesforce** — CRM data access
- **1Password** — secret retrieval
- **Slack & Telegram** — messaging integration
- **Outlook** — email access

## Building the GitLab Skill

Today's milestone was the GitLab skill. It's a good example of what building a skill actually looks like.

The skill wraps `glab` (GitLab's official CLI) and exposes subcommands for the things I actually need: listing projects, viewing merge request details, checking pipeline status, searching issues. Each command fetches data from the GitLab API and formats it in all three output modes.

What made this interesting was the collaboration model. I described what I needed — "I want to query merge requests for a project and see who's reviewing what" — and the AI handled the implementation details: argument parsing, API pagination, output formatting. I focused on *what questions I want to ask my codebase*, and the agent focused on *how to get the answers*.

This is the part that surprises people: it's not about the AI writing code autonomously. It's about having a conversation where you think out loud and the agent fills in the gaps. It's pair programming where your partner has perfect recall of API documentation and zero opinions about tabs vs. spaces.

## What's Next

The skills exist, but the agent doesn't yet *think*. Right now each skill is a standalone CLI tool. The next phase is building the orchestration layer — the part where the agent can chain skills together to answer compound questions like "Show me all open MRs from this sprint that touch the authentication module, and cross-reference with the Jira tickets they're linked to."

That's where it gets interesting. And that's what I'll write about next.

---

*This blog is itself part of the CTO-Agent project — built with Jekyll, hosted on GitHub Pages, and written in the same repo where the agent lives. Meta? Maybe. But if you're going to build an AI agent, you might as well let it help you write about building it.*
