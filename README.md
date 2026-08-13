# Limn

> An agent-native deploy platform — build the whole app, let the agent ship it.

**Limn** is a CLI and runtime for building and deploying small full-stack TypeScript
apps — **capsules** — without leaving your coding agent. It is the third generation of
the "deploy from your agent" idea (postplan → lakebed → **limn**), rebuilt model- and
provider-agnostic, with a plugin/extension market and telemetry built in from day one.

## Why "Limn"?

A lakebed sits at the bottom of a lake. **Limn** — from *limnology*, the study of inland
waters — is what you do at the surface: you *limn* (delineate, sketch, describe) the
thing before it exists. Same terrain, different layer. We aren't stealing the name,
we're building the next layer of it.

## The capsule

```
server/index.ts      # schema, queries, mutations, endpoints
client/index.tsx     # Preact UI
shared/              # pure TypeScript shared by both sides
.env.lakebed.server  # server-only secrets
```

The whole app is one directory. The runtime ships the boring parts — database, auth,
object storage, logs, deploys — so an agent can go from idea to a live URL in one session.

## Commands

```sh
npx limn new my-app --template todo     # scaffold
npx limn dev                            # run locally
npx limn deploy                         # live URL, anonymous first
npx limn claim                          # attach it to your account
npx limn db dump <url>                  # inspect live state
npx limn logs <url>                     # tail live logs
npx limn domains add my-app.limn.app    # name your subdomain
```

## Vision

See [docs/vision.md](docs/vision.md). The long game: a model-agnostic harness with a
plugin market — Raycast-style extensions, home-automation integrations, one-off
generators, telemetry — where the UI is optional and the CLI is the API.

## Research

Curated subdomain/intel notes on the incumbent stack (10xn.dev, postplan.dev,
lakebed.dev, lakebed.app):

- [docs/research/t3-ecosystem-subdomains.md](docs/research/t3-ecosystem-subdomains.md)

## Status

Early alpha. Static landing + research live at <https://limn.pages.dev> (GitHub Pages).
Dogfooding daily.

## License

MIT