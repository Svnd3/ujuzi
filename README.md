# Universal Skills

A small, frontend-focused skill pack for AI coding agents. Twelve skills that cover the full loop: understand the product, explore directions, build the interface, verify it against the design, and audit the experience — including accessibility.

No API keys. No bundled binaries. No vendor lock-in. Each skill is a plain folder with a `SKILL.md`.

> **Repo name tip:** GitHub repo names cannot contain spaces. Rename this folder to `universal-skills` when you publish.

---

## What's inside

### Understand

| Skill | What it does |
| --- | --- |
| [`get-context`](skills/get-context/SKILL.md) | Mandatory design-brief gate. Clarifies the product, audience, and outcome before any UI work starts. |
| [`research`](skills/research/SKILL.md) | Source-grounded UX research on real user pain, friction, onboarding, docs, and support issues. |

### Explore

| Skill | What it does |
| --- | --- |
| [`ideate`](skills/ideate/SKILL.md) | Generates image-based design directions, remixes, and visual alternatives from a brief. |
| [`image-to-code`](skills/image-to-code/SKILL.md) | Turns a mockup, screenshot, or generated concept into a faithful working frontend. |
| [`url-to-code`](skills/url-to-code/SKILL.md) | Recreates a live page you own as a runnable, frontend-only local app. |

### Build

| Skill | What it does |
| --- | --- |
| [`frontend-app-builder`](skills/frontend-app-builder/SKILL.md) | Builds polished frontends from scratch with a concept-first, browser-tested workflow. |
| [`react-best-practices`](skills/react-best-practices/SKILL.md) | 64 prioritized React/Next.js rules for rendering, data fetching, and bundle performance. |

### Verify

| Skill | What it does |
| --- | --- |
| [`design-qa`](skills/design-qa/SKILL.md) | Compares the source design to the rendered implementation and returns a prioritized fix list. |
| [`frontend-testing-debugging`](skills/frontend-testing-debugging/SKILL.md) | Browser-driven QA loop for layout regressions, console errors, responsive behavior, and visual checks. |

### Audit and accessibility

| Skill | What it does |
| --- | --- |
| [`audit`](skills/audit/SKILL.md) | Screenshot-first audit of a flow, funnel, onboarding path, or screen with UX, design, and accessibility findings. |
| [`accessibility-and-inclusive-visualization`](skills/accessibility-and-inclusive-visualization/SKILL.md) | Color contrast, redundant encoding, keyboard paths, screen-reader text, and export-safe fallbacks. |
| [`visualization-strategy-and-critique`](skills/visualization-strategy-and-critique/SKILL.md) | Chooses the right visual form and critiques charts, dashboards, and diagram layouts. |

---

## How the skills fit together

```text
get-context  →  ideate / image-to-code / url-to-code  →  frontend-app-builder
     │                                                        │
     └──────────────────────► audit ◄──────────── design-qa ◄──┘
                              │
                    accessibility + visualization review
```

A typical pass: clarify the brief, pick or generate a direction, build it, run design QA against the source, then audit the flow for UX and accessibility issues.

---

## Install

**Fastest path (Ubuntu):** copy the block in [`INSTALL.md`](INSTALL.md) into a terminal and press Enter. It downloads this repo and installs every skill automatically — no `git`, no `sudo`, no editing.

Each skill is self-contained. You can also copy the folders into the skills directory your agent reads.

**Codex**

```bash
mkdir -p ~/.codex/skills
cp -r skills/* ~/.codex/skills/
```

**Claude Code**

```bash
mkdir -p ~/.claude/skills
cp -r skills/* ~/.claude/skills/
```

**Windows PowerShell**

```powershell
Copy-Item -Recurse "skills\*" "$env:USERPROFILE\.codex\skills\"
```

**Any other harness**

Point the agent at `skills/`, or copy individual skill folders into whatever directory it scans for `SKILL.md` files.

---

## Layout

```text
universal-skills/
├── README.md
├── INSTALL.md                 # one-shot copy-paste installer for Ubuntu
├── manifest.json              # machine-readable skill inventory
├── THIRD-PARTY-NOTICES.md     # provenance and attribution
└── skills/
    ├── audit/
    │   ├── SKILL.md
    │   ├── agents/openai.yaml
    │   └── references/design-audit-framework.md
    ├── accessibility-and-inclusive-visualization/
    ├── design-qa/
    ├── frontend-app-builder/
    ├── frontend-testing-debugging/
    ├── get-context/
    ├── ideate/
    ├── image-to-code/
    ├── react-best-practices/
    ├── research/
    ├── url-to-code/
    └── visualization-strategy-and-critique/
```

---

## Compatibility notes

- **Agent-agnostic.** Skills rely on `SKILL.md` frontmatter plus Markdown instructions.
- **Browser automation is optional but recommended.** Audits and design QA are evidence-first; they work best when the agent can capture screenshots (Browser tool, Playwright, or similar).
- **Image generation is optional.** `ideate` and `image-to-code` benefit from an image model but degrade gracefully to provided references.
- **No network calls or credentials** are embedded in this pack.
- **`design-qa` needs two artifacts:** a source visual target and a rendered implementation. It blocks handoff when either is missing.

---

## Scope

This pack is deliberately focused. It covers product UX, interface design, frontend implementation, verification, and accessibility — the work most teams repeat on every project.

It intentionally does **not** include:

- Figma, Canva, or Adobe production pipelines
- Deep chart-engineering and 3D/WebGL skills
- Native iOS, Android, or Expo UI packs
- Cloud, backend, commerce, messaging, or finance/life-science domain packs

Keeping the free pack tight leaves room for deeper, specialized packs to stand on their own.

---

## Editing

- Keep the `name` and `description` fields in frontmatter intact; agents route on them.
- Keep relative links inside the skill folder. A skill should never depend on a sibling that might not be installed.
- Add or remove skills, then regenerate `manifest.json` or update it by hand.

---

## License and attribution

Read [`THIRD-PARTY-NOTICES.md`](THIRD-PARTY-NOTICES.md) before publishing.

- These files were adapted from an installed skill library. Confirm you hold redistribution rights for each one.
- `react-best-practices` is attributed to Vercel Engineering in its own metadata and had no license file in the source copy.
- Add your chosen `LICENSE` (MIT or Apache-2.0 are common for skill packs) and keep the notices file in place.
