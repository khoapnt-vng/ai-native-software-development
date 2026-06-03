# superpowers-plus-greennode-agentbase

**Build, spec, and deploy AI agents — one suite.** This Claude Code plugin
combines three skill families so you can take an AI agent from idea to
production on GreenNode AgentBase:

- **superpowers** — brainstorming, planning, TDD, debugging, code review (the build muscle)
- **spec-driven-development** — persistent specs (constitution + feature specs) that anchor intent
- **GreenNode AgentBase** — scaffold, deploy, and operate agents on GreenNode infrastructure

Forked from [obra/superpowers](https://github.com/obra/superpowers) (MIT,
© Jesse Vincent); the AgentBase skills are © GreenNode, bundled unmodified. See
[`CREDITS.md`](CREDITS.md) and [`LICENSE`](LICENSE).

Skills are invoked as `/superpowers-plus-greennode-agentbase:<skill-name>` (e.g.
`/superpowers-plus-greennode-agentbase:agentbase-wizard`) or simply by describing your task
— the skills auto-trigger from natural language.

> 📖 **Prefer a visual tour?** Open [`docs/superpowers_plus_agent_base.html`](docs/superpowers_plus_agent_base.html) in a
> browser for an illustrated introduction — or the 5-slide
> [webinar deck](docs/webinar-deck.html) for a quick overview.

---

## What is it?

`superpowers-plus-greennode-agentbase` is a **skills package** for AI coding agents (Claude
Code, and other harnesses via the bundled manifests). Skills are reusable
instruction sets the agent loads on demand. Install once and your agent gains 23
skills that cover the **entire lifecycle of building an AI agent**:

1. **Spec** the work so intent is written down, not lost in chat.
2. **Build** it with disciplined engineering (TDD, planning, review).
3. **Deploy** and operate it on GreenNode AgentBase.

The skills trigger automatically at the right moments — you mostly just describe
what you want.

## Objective

AI agents drift. Across a long session the "why" lives only in the chat history
— lost on clear, lost when a new session starts, corrupted as context grows.
Agents jump to code before the problem is understood, invent tech stacks
silently, and produce work no one committed to reviewing. And once something is
built, getting it *deployed* is a separate scramble.

This suite fixes both ends:

- **Spec-Driven Development** keeps a persistent spec as the source of truth —
  *the spec is the brain; the agent is the muscle.* You stay the architect and
  reviewer.
- **GreenNode AgentBase** turns "it works on my machine" into a deployed,
  monitored agent with a few prompts.

## Who is it for, and what can you build?

**For:** developers, product managers, and teams who use an AI coding agent and
want consistent, shippable results — especially anyone deploying agents to
**GreenNode AgentBase**.

**Build things like:**
- A **chatbot / assistant** (LangChain or LangGraph) with memory.
- A **RAG or tool-using agent** that calls external APIs (with managed identity & secrets).
- An **internal automation agent** specced, tested, and deployed to GreenNode.
- Or any software project — the superpowers + SDD half works for non-agent apps too.

**Do this with it:** spec a feature, plan it into bite-sized tasks, implement it
test-first, review it, then scaffold/deploy/monitor it on GreenNode — all guided.

---

## Installation (this plugin)

### Claude Code — try it for one session (no install)

Loads the plugin only for the current session. Great for testing; no conflict
with any plugin you already have.

```bash
git clone https://github.com/khoapnt-vng/superpowers-plus-greennode-agentbase.git
cd <your-project-folder>
claude --plugin-dir /absolute/path/to/superpowers-plus-greennode-agentbase
```

### Claude Code — install permanently

```bash
claude plugin marketplace add khoapnt-vng/superpowers-plus-greennode-agentbase
claude plugin install superpowers-plus-greennode-agentbase@superpowers-plus-greennode-agentbase-dev
# restart Claude Code
```

> **Already have the official `superpowers` plugin installed?** This suite is a
> *superset* (it contains all 14 superpowers skills). Disable the official one
> first to avoid duplicate-skill collisions:
> ```bash
> claude plugin disable superpowers
> ```
> Undo anytime: `claude plugin uninstall superpowers-plus-greennode-agentbase` then
> `claude plugin enable superpowers`.

### Other harnesses

The repo ships manifests for Codex (`.codex-plugin/`), Cursor (`.cursor-plugin/`),
Gemini (`gemini-extension.json`), and OpenCode (`.opencode/`). Point each tool's
plugin/extension installer at this repo
(`https://github.com/khoapnt-vng/superpowers-plus-greennode-agentbase`). Install per
harness if you use more than one.

### Verify it loaded

In a session, type `/` and look for `superpowers-plus-greennode-agentbase:…` entries, or
just say *"set up a spec for a new agent"* and watch a skill trigger.

---

## Step-by-step: build → spec → deploy an agent

A complete walk-through. Each numbered step is one prompt to your agent; the
skill named in **bold** triggers automatically.

### 1. Spec it  →  `spec-driven-development`
> *"I want to build a customer-support agent. Set up the spec."*

The agent checks for a project **constitution** (`docs/specs/mission.md`,
`tech-stack.md`, `roadmap.md`). If none exists it interviews you (greenfield) or
reads your code (legacy), then writes a per-feature spec:
`docs/specs/<feature>/plan.md`, `requirements.md`, `validation.md` — every
acceptance item carrying a runnable `verify:` check.

### 2. Refine the design  →  `brainstorming`
> *"Let's design the agent's conversation flow."*

Socratic Q&A, alternatives with trade-offs, a design you approve in chunks.

### 3. Plan the work  →  `writing-plans`
> *"Turn the spec into an implementation plan."*

Bite-sized tasks (2–5 min each) with exact files, code, and verification steps.

### 4. Build it  →  `subagent-driven-development` + `test-driven-development`
> *"Execute the plan."*

Fresh subagent per task, RED-GREEN-REFACTOR, two-stage review (spec compliance,
then code quality) between tasks.

### 5. Set up the platform  →  `agentbase-wizard`
> *"Scaffold this as a GreenNode agent using LangGraph."*

Scaffolds the agent project (LangChain/LangGraph templates), wires memory
(`agentbase-memory`), identity & secrets (`agentbase-identity`), and LLM access
(`agentbase-llm`).

### 6. Deploy it  →  `agentbase-deploy`
> *"Deploy my agent to GreenNode."*

Builds & pushes the image to the container registry, creates/updates the runtime,
and verifies the endpoint.

### 7. Operate it  →  `agentbase-monitor` / `agentbase-teardown`
> *"Show my agent's logs"* … *"Tear down all resources for this project."*

Logs, metrics, scaling, and one-shot cleanup when you're done.

### 8. Keep specs in sync (throughout)
Ask the agent to make code changes so it updates the related spec at the same
time — code and spec never drift apart.

---

## What's inside (23 skills)

### Spec-Driven Development
- **spec-driven-development** — Persistent SDD specs: a project constitution (mission/tech-stack/roadmap) + per-feature specs (plan/requirements/validation); adaptive greenfield interview or legacy reverse-engineering; standalone core with optional superpowers hooks.

### GreenNode AgentBase (build & deploy on GreenNode)
- **agentbase** — Platform reference & getting-started guide
- **agentbase-wizard** — Guided agent build from scaffold to deploy (LangChain/LangGraph templates)
- **agentbase-deploy** — Deploy, manage runtimes, and container registry (vCR)
- **agentbase-identity** — Agent identities and outbound auth providers
- **agentbase-llm** — Platform LLM model access and API keys
- **agentbase-memory** — Conversation history and long-term memory
- **agentbase-monitor** — Logs, metrics, and status for deployed agents
- **agentbase-teardown** — Remove all platform resources for a project

### Build & engineering (superpowers core)
- **brainstorming** — Socratic design refinement
- **writing-plans** — Detailed implementation plans
- **executing-plans** — Batch execution with checkpoints
- **subagent-driven-development** — Fast iteration with two-stage review
- **test-driven-development** — RED-GREEN-REFACTOR cycle
- **systematic-debugging** — 4-phase root-cause process
- **verification-before-completion** — Ensure it's actually fixed
- **dispatching-parallel-agents** — Concurrent subagent workflows
- **requesting-code-review** / **receiving-code-review** — Review workflow
- **using-git-worktrees** — Parallel development branches
- **finishing-a-development-branch** — Merge/PR decision workflow
- **writing-skills** — Create new skills following best practices
- **using-superpowers** — Introduction to the skills system

## Philosophy

- **The spec is the brain; the agent is the muscle** — anchor intent in persistent specs.
- **Test-Driven Development** — write tests first, always.
- **Simplicity first** — the minimum that solves the problem; YAGNI.
- **Surgical changes** — touch only what the request requires.
- **Evidence over claims** — verify before declaring success.

## Credits & License

This is a fork of [obra/superpowers](https://github.com/obra/superpowers) (MIT,
© Jesse Vincent) with two additions: the original `spec-driven-development` skill
(© khoapnt-vng) and GreenNode's AgentBase skills (© GreenNode, bundled
unmodified). Full attribution in [`CREDITS.md`](CREDITS.md). The GreenNode
AgentBase user guide is preserved at
[`docs/greennode-agentbase-README.md`](docs/greennode-agentbase-README.md).

MIT License — see [`LICENSE`](LICENSE).
