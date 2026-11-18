# Third-Party Notices

This file records where the contents of this repository came from and what must be checked before publishing it publicly.

## Provenance

Files were copied on **2026-11-16** from an installed skill library, from these source groups:

| Source group | Skills taken |
| --- | --- |
| `product-design` | `audit`, `design-qa`, `get-context`, `ideate`, `research`, `image-to-code`, `url-to-code` |
| `build-web-apps` | `frontend-app-builder`, `frontend-testing-debugging`, `shadcn` |
| `build-web-data-visualization` | `accessibility-and-inclusive-visualization`, `visualization-strategy-and-critique` |

## Attribution

- No skill in this pack declares an `author` or `license` field. All twelve were authored as agent instructions rather than copied from a licensed documentation set.
- **`shadcn`** documents the shadcn/ui command line and component conventions. shadcn/ui itself is distributed under the MIT license; this skill contains workflow guidance and no redistributed library code.
- The Vercel-attributed `react-best-practices` skill was intentionally left out of this pack, so no third-party rule text is reproduced here.

## Modifications made for this pack

- Removed empty plugin-internal sections ("Critical Overrides", router/preflight instructions) that referenced files not included here.
- Rewrote links that pointed to skills or shared references outside this subset into plain-language guidance.
- Trimmed reference lists to files that actually exist in each skill folder.
- Replaced a cross-reference to the removed `react-best-practices` skill with an in-pack pointer.

No substantive guidance or workflows were changed.

## Before you publish

1. Confirm you have the right to redistribute every file in `skills/`.
2. Choose a license and add a `LICENSE` file at the repository root.
3. Keep this notice in the repository, and extend it if you add third-party material.
