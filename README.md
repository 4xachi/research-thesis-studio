# Research & Thesis Studio

An agent skill for developing research papers and theses with defensible methods, traceable evidence and clear academic writing.

**Skill name:** `create-research-paper` · **Version:** 1.0.0 · **License:** MIT

## What it does

- Selects an appropriate route for quantitative, qualitative, mixed methods, systematic/scoping reviews, design science, IT capstones and other scholarly work.
- Assists with research questions, proposals, literature reviews/RRL, instruments, methodology, analysis plans, findings, discussion and defense preparation.
- Separates source identity, content actually read and correction/retraction checks. Links claims to source IDs and specific evidence locations.
- Adapts to institutional manuals and journal requirements. Uses APA 7 provisionally when no style is specified; follows other requested citation styles.
- Provides academic editing that preserves meaning, qualifications, numbers and citations, with optional Humanizer/Unslop integration.
- Includes a 22-publication retrieval index, a project-state template and an offline structural checker.

The skill supplies instructions and reference material. It does not train model weights, perform research without tools/data, guarantee publication, or guarantee human-authorship/detector scores. The checker does not browse, prove source authenticity, or establish that a cited passage supports a claim. Human review and actual evidence remain necessary.

## Quick start

1. Read [SETUP.md](SETUP.md) for complete installation, usage and GitHub publication instructions.
2. Install the entire `skills/create-research-paper` folder in your agent's skill location.
3. Invoke `create-research-paper` in that agent and provide your topic, school/journal requirements, existing work and available data.

Example request:

> Use create-research-paper to develop a proposal about a network-support portal. Determine the appropriate research design, find verifiable sources, and follow my school manual. No data have been collected; keep findings as empty table shells.

After publishing this repository, users can install through the optional Skills CLI:

```bash
npx skills add OWNER/research-thesis-studio --skill create-research-paper
```

Replace `OWNER` with the actual GitHub owner. The interactive installer lets users select their agent. The manual installation needs no Node.js.

## Package contents

| Path | Purpose |
|---|---|
| `skills/create-research-paper/SKILL.md` | Agent entry point and workflow |
| `skills/create-research-paper/references/` | Seven focused research and writing guides |
| `skills/create-research-paper/assets/research-state.json` | Blank project-state template |
| `skills/create-research-paper/scripts/research_check.py` | Python structural audit with built-in self-tests |
| `examples/` | Ready-to-use prompts and a fictional proposal-state example |
| `SETUP.md` | Setup, installation, use, maintenance and publication |
| `tools/package.py` | Validate and build reproducible release ZIPs |
| `tests/test_release.py` | Packaging smoke and regression tests |
| `.github/workflows/validate.yml` | GitHub validation on pushes and pull requests |
| `THIRD_PARTY_NOTICES.md` | External references, optional integrations and scope |

## Check and build

Use Python 3.10 or newer. No pip packages, API keys or server are required for these commands. From this repository's root:

```bash
python -B tools/package.py --check
python -B -m unittest discover -s tests -v
python -B tools/package.py --build
```

The build creates a GitHub repository ZIP, a standalone skill ZIP and `SHA256SUMS` in `dist/`. `dist/` is excluded from source archives. Builds use fixed ZIP timestamps and an explicit file list.

The bundled source and checker have been exercised locally on Python 3.12. The workflow is configured for Python 3.10 and 3.12; those hosted jobs run after the repository is uploaded. Live Codex/Claude installations, the optional CLI and GitHub Actions were not executed as part of this export.

## Research integrity

The skill prohibits fabricated references, DOIs, participant quotations, results, approvals and claims of searches that were not performed. It distinguishes proposals, real analysis and synthetic demonstrations. It preserves uncertainty, contradictory evidence and appropriate conclusion boundaries. Follow the institution/journal's actual assistance and disclosure rules.

Humanizer and Unslop are optional external skills, not bundled dependencies. Built-in academic editing works without them. No automatic third-party downloads or external uploads occur.

## License and contributions

Original package content is released under [MIT](LICENSE). External publications and projects retain their own rights; see [notices](THIRD_PARTY_NOTICES.md). Changes should keep evidence integrity intact and pass the documented checks. Add new distributed files to the explicit manifest in `tools/package.py`.
