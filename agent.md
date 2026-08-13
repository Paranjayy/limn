# Project Agent Contract (agent.md)

## Mission

Help deliver a clean, model-agnostic CLI and deployment harness for agent-native capsules, while ensuring rock-solid verification, simple structures, and no feature regressions.

## Current Stage

**Prototype / Scaffolding**

Optimize primarily for:
1. Legible capsule specifications and clean research documentation.
2. Bulletproof local compilation and deployment safety net.
3. Zero-fluff visual execution of interfaces.

Do not optimize prematurely for:
- Scale issues (imaginary millions of deployments).
- Custom cloud runtimes (stay on Vercel/Railway for now).

## Sources of Truth

- Vision & Scope: `docs/vision.md`
- Design Playbook: `docs/design.md`
- Ecosystem Intel: `docs/research/t3-ecosystem-subdomains.md`
- Commands: This file.

## Repository Map

- `/index.html`: Main landing page (Vercel-deployed).
- `/docs/`: Vision, design, and research folders.
- `/vercel.json`: Vercel config.
- `/.github/workflows/pages.yml`: GitHub Pages deployment.

## Non-Negotiable Invariants

- **Zero Secrets**: No tokens, API keys, or credentials enter commit logs or client bundles.
- **Dogfood or Don't Ship**: Every feature added must be usable immediately by another agent or developer locally.
- **Strict Backward Compatibility**: New tools (like Safari clean lists) must coexist cleanly with previous indexes.

## Standard Workflow

1. Inspect relevant code, files, and local instructions.
2. State assumptions and clarify ambiguities.
3. Propose a plan showing exactly which files will change.
4. Implement using precise file replacements (avoid broad writes where possible).
5. Run verification (e.g., node syntax compiler, static checkers).
6. Verify deployment build locally.
7. Record findings and update the repository map.

## Commands

- Serve locally: `python3 -m http.server 8000`
- Check syntax: `node -c [file]`

## Verification Policy

| Change | Minimum Evidence |
|---|---|
| Docs/Markdown | Format & link checks |
| Vanilla JS | `node -c [file]` validation |
| Static UI | Local HTTP server launch + inspection |
| Deployment configs | JSON format lint |

## Permission and Safety Policy

Allowed without asking:
- Read/search all files.
- Run static checks and builds.
- Modify files in scope (docs, saptak, limn landing tools).

Ask before:
- Destructive git actions (hard resets, force pushes).
- Modifying repo secrets.
- Creating new repositories.
