# Limn — Vision

**Limn** is the third generation of "deploy from your agent" tooling, rebuilt from
first principles. It replaces neither the model nor the IDE — it replaces the
**everything around them**.

## The problem

Today an agent lives in a graveyard of one-off harnesses: this CLI for deploys, that
one for code review, another for home automation, an extension over here, a webhook
over there. Each has its own auth, its own storage, its own docs written for humans.
Nobody owns the **agent's OS** — the single, model-agnostic layer where a capability
is one command away.

## The shape of Limn

### 1. Capsules — full-stack apps, one directory

A capsule is the unit of deploy. Server file, client file, shared types, an env file.
The runtime provides DB, auth, storage, logs, and a URL. The contract is small enough
that an agent holds the entire app in context and can ship it in one session.

### 2. Model & provider agnostic

No "Claude-native", no "Codex-only". Limn is a CLI + runtime + API. Any harness —
Claude Code, Codex, Cursor, OpenClaw/Hermes-style agents, your own — can build and
deploy. The docs ship `llms.txt`, `docs.json`, and raw markdown precisely so *machines*
read them first.

### 3. Plugin / extension market

The store is the product. Raycast-style extensions, Chrome-style add-ons, macOS-style
preferences — everything is an installable plugin:

- **Integrations**: AC/bulb control, home automation, services, webhooks.
- **Generators**: true generative features — one-off tools with a seamless flow, not
  a mess of tabs.
- **Telemetry**: first-class telemetry for agents (the thing nobody else ships — a
  record of every run, deploy, and decision, queryable).
- **Messaging**: bots in Telegram (topics/channels), Discord, and wherever threads
  exist — a channel is just another plugin surface.

### 4. Observability by default

Every capsule logs. Every deploy is inspectable from the CLI: `db dump`, `db export`,
`logs`. State is first-class, not a black box. History is queryable — what was built,
when, by which agent, with what result.

### 5. Cheap enough to throw away

Anonymous deploys expire. Unclaimed work is deleted. Building throwaway one-off things
is the point — and it should cost nothing.

## The principles

1. **Agents are the users.** Human-facing UI is optional; the CLI is the API.
2. **Small surface.** One entry point per side. Fewer moving parts means agents build
   reliably.
3. **Inspect before you guess.** DB dumps, log tails, state exports — always available.
4. **Never lock a model.** The contract is the product; providers are interchangeable.
5. **Deploy continuously.** Every push is a deploy. Dogfood or don't ship.

## Roadmap

- [x] Static landing + ecosystem research (this repo)
- [ ] CLI: `new` / `dev` / `build` scaffold against a local runtime
- [ ] Runtime: in-memory DB + query/mutation contract
- [ ] Deploy API + subdomain router (`*.limn.app`)
- [ ] Auth (guest → Google), storage, logs
- [ ] `db dump` / `logs` inspection over the wire
- [ ] Plugin registry + `limn install`
- [ ] Telemetry stream + queryable history
- [ ] Messaging plugins (Telegram topics, Discord)

## Adjacent work

- **Saptak** — a web harmonium / instrument studio (separate repo), our playground for
  shipping delightful static apps on Limn.
