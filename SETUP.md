# Setup, installation and publication

This guide covers release 1.0.0 of Research & Thesis Studio. The agent skill's internal name is `create-research-paper`.

## 1. Choose the ZIP

| File | Use |
|---|---|
| `research-thesis-studio-v1.0.0.zip` | Complete repository: documentation, skill, examples, validation workflow and build tools |
| `create-research-paper-v1.0.0.zip` | Standalone skill: one `create-research-paper/` directory with SKILL.md, guides, assets, checker and license |

Extract the repository ZIP and open its `research-thesis-studio` folder. The README must be at the repository root when uploading to GitHub. The standalone ZIP is for hosts that explicitly support skill ZIP imports, or for extracting and copying into a local agent directory. ZIP-import availability depends on the host; no universal upload button is assumed.

## 2. Requirements

- An AI agent that can load Agent Skills or read a skill directory and its referenced files.
- Python 3.10+ for the optional offline checker and release tools. No pip dependencies.
- Web/scholarly-search access for live source discovery and verification; access to the actual paper text where required. Without it, the skill labels limitations and uses supplied material provisionally.
- Access to your actual drafts, school/journal instructions and data for the requested work.

Git is needed only for cloning/publishing. Node.js 22.20+ and internet access are needed only for the optional Skills CLI. An agent's model/service may have its own subscription or API requirements; this package does not include or require a particular model account.

On Windows, use `python`, or substitute `py -3` if that is your Python launcher. On macOS/Linux, substitute `python3` if `python` is unavailable.

## 3. Install manually: Codex CLI / IDE

Codex discovers local skills under `.agents/skills`. For a personal installation, copy the complete skill folder to `~/.agents/skills/create-research-paper`. For a project-only installation, use the project's `.agents/skills/create-research-paper` directory. See the [official skill-location guide](https://learn.chatgpt.com/docs/build-skills).

From the extracted repository on macOS/Linux:

```bash
mkdir -p "$HOME/.agents/skills"
cp -R ./skills/create-research-paper "$HOME/.agents/skills/"
```

These first-install commands assume the destination skill does not already exist. Back up an existing installation and follow the update section before replacing it.

Windows PowerShell, from the extracted repository:

```powershell
$skillDestination = Join-Path $env:USERPROFILE '.agents\skills\create-research-paper'
if (Test-Path $skillDestination) { throw 'Skill already exists; back it up before updating.' }
New-Item -ItemType Directory -Force -Path (Split-Path $skillDestination) | Out-Null
Copy-Item -Recurse '.\skills\create-research-paper' $skillDestination
```

Confirm that `create-research-paper/SKILL.md` is directly inside the installed directory, not nested under a second same-named folder. In Codex, use `$create-research-paper` or find it with `/skills`. If the selector appears stale, refresh/reopen its Skills view or start a new session.

## 4. Install manually: Claude Code

Copy the same complete skill folder to `~/.claude/skills/create-research-paper` for personal use, or `.claude/skills/create-research-paper` for one project. Invoke `/create-research-paper`. These are local Claude Code locations; cloud/account skill installation is separate. See [Claude Code's official guide](https://code.claude.com/docs/en/skills).

macOS/Linux, from the repository:

```bash
mkdir -p "$HOME/.claude/skills"
cp -R ./skills/create-research-paper "$HOME/.claude/skills/"
```

On Windows, use the PowerShell commands above with `.claude\skills\create-research-paper` as the destination. Use the same first-install and folder-layout checks.

## 5. Optional Skills CLI installation

The third-party [Skills CLI](https://github.com/vercel-labs/skills) can discover this repository's `skills/` directory. After publication, replace `OWNER` with the real GitHub owner:

```bash
# Preview available skills
npx skills add OWNER/research-thesis-studio --list

# Personal Codex installation
npx skills add OWNER/research-thesis-studio --skill create-research-paper --agent codex --global

# Personal Claude Code installation
npx skills add OWNER/research-thesis-studio --skill create-research-paper --agent claude-code --global
```

Before publication, run the same commands with `./` as the source from this repository. Omit `--global` for project scope. Review the installer's choices. This export does not install the CLI or execute it on your machine. Other agent compatibility depends on the target host and its supported skill locations.

## 6. Start a research project

Provide the topic, discipline/degree level, intended output, required style, school manual or journal requirements, existing drafts, available sources/data and current stage. Supply only what is available; the agent should not stall for nonessential details.

Example:

> Use create-research-paper. I need Chapters 1–3 for an IT capstone about a network-support portal. We have not collected data. Use APA 7 provisionally and my attached school manual. First check the question/design alignment, then build a verified source register and a working proposal.

The skill routes research design, searches and verifies sources, plans methods, drafts supported sections, analyzes actual supplied data when appropriate, edits for clarity, and audits the result. It should ask only blocking questions. For one small edit, it should not launch a full thesis process.

See [example prompts](examples/prompts.md) for RRL, qualitative design, mixed methods, citation audits and revision requests. A completed paper requires real evidence; this package cannot manufacture missing data or perform unexecuted procedures.

## 7. Use the offline checker

Keep project files outside this distribution. From the repository, create a separate project directory and copy the blank state:

```bash
mkdir ../research-project
cp skills/create-research-paper/assets/research-state.json ../research-project/research-state.json
python -B skills/create-research-paper/scripts/research_check.py ../research-project/research-state.json
```

PowerShell uses `New-Item -ItemType Directory ..\research-project` and `Copy-Item` for the first two operations, keeping the same parent-directory destination. The Python command is the same.

The blank state intentionally reports unresolved/empty-state warnings. Fill it according to [the state contract](skills/create-research-paper/references/research-state.md). A fictional populated proposal example is in [examples/proposal-state.json](examples/proposal-state.json); it is not a real study or a verified bibliography.

```bash
python -B skills/create-research-paper/scripts/research_check.py --self-test
python -B skills/create-research-paper/scripts/research_check.py examples/proposal-state.json
python -B skills/create-research-paper/scripts/research_check.py ../research-project/research-state.json --submission
```

Exit codes: 0 means no structural errors, 1 means audit errors, and 2 means invalid invocation/unreadable JSON. Warnings can accompany exit 0. Submission mode makes unresolved manuscript claims and unchecked cited-source integrity errors. A proposal can contain planned statements; a completed study needs actual findings. A clean state file does not prove citation entailment, source existence, statistical validity or a complete manuscript. The checker does not parse the manuscript automatically.

When running from another working directory, use the checker's absolute installed path. Resolve that path from the actual SKILL.md location instead of assuming `scripts/` is under the current directory.

## 8. Humanizer and Unslop

The built-in academic-editing guide is always available. Separate Humanizer/Unslop skills are optional and are not included or installed by this package. If present, the agent reads their actual instructions and applies compatible editing while preserving research meaning and evidence.

Public projects to inspect before installing:

- [Humanizer by blader](https://github.com/blader/humanizer).
- [Unslop prose editor by westonhancock](https://github.com/westonhancock/unslop).
- [Unslop domain-profile workflow by mshumer](https://github.com/mshumer/unslop), a different tool that may require its own runtime.

Choose the intended implementation and follow its current installation instructions. No separate plugin is required for the bundled fallback. Neither editing route guarantees Turnitin outcomes or establishes human authorship. Follow your actual institution's disclosure policy.

## 9. Publish on GitHub

Create a new empty repository named `research-thesis-studio` under your account. Do not initialize a second README/license if uploading this complete package. Choose the visibility you intend. No GitHub publication has been performed by this export.

**Browser route:** extract the repository ZIP, open its inner `research-thesis-studio` directory, and upload its contents into the repository root. Include `.github`, `.gitignore` and `.gitattributes`; file pickers may hide dot-prefixed items. Confirm README.md and `skills/create-research-paper/SKILL.md` appear at the expected paths.

**Git route:** from the extracted repository, after creating the empty remote, replace `OWNER` below:

```bash
git init -b main
git add .
git commit -m "Release Research & Thesis Studio v1.0.0"
git remote add origin https://github.com/OWNER/research-thesis-studio.git
git push -u origin main
```

Use your normal GitHub authentication. The package needs no stored token or credentials. Suggested repository description: “Research and thesis agent skill with study-design guidance, source verification, citations and academic editing.” Suggested topics: `agent-skills`, `research`, `thesis`, `academic-writing`, `citations`.

Run local validation and tests before pushing:

```bash
python -B tools/package.py --check
python -B -m unittest discover -s tests -v
python -B tools/package.py --build
```

The included workflow runs checks/tests on pushes, pull requests and manual dispatch when GitHub Actions is enabled. Its hosted runs start only after publication. Source validation, archive checks and tests were run locally on Python 3.12; live agent installations and remote CI remain environment-specific checks.

Create a GitHub Release tagged `v1.0.0` from your intended branch and attach both ZIPs and `SHA256SUMS` from `dist/`. Describe the capabilities and limitations accurately. Updating README install examples from `OWNER` to your account is optional for packaging but necessary for a working public copy/paste example. This repository does not include a marketplace registration or publish a plugin listing automatically.

## 10. Update, customize and uninstall

Keep real research files outside the installed skill. Before updating, back up customized skill files and project state. Replace the installed `create-research-paper` directory with the new whole folder; avoid copying one same-named directory inside another. For CLI-managed installations, use the CLI's current update/remove commands.

For a manual uninstall, remove only the `create-research-paper` directory from the location where you installed it. Keep project drafts and state. If you have the skill through an account/plugin manager, disable or uninstall it there instead of deleting a managed cache.

Customize the guides for a discipline or institutional format without removing integrity checks. Put per-project manuals and data in the project, not in the public skill. Add new distributed files to the `SKILL_FILES`/`REPO_FILES` manifest in `tools/package.py`. Before a new version, update VERSION, version labels/examples, run tests, build fresh archives and verify their contents. The MIT license covers original package content; external sources retain their own rights.

## 11. Troubleshooting

| Problem | Check |
|---|---|
| Skill missing from selector | Correct host directory, one skill folder level, exact SKILL.md capitalization, enabled host/account support; refresh/reopen Skills view |
| Duplicate skill entries | More than one installation with the same name; keep the intended scope |
| Script not found | Use the actual installed skill path, not the current project directory |
| Python not found | Install Python 3.10+; try python3 or py -3 |
| Cannot verify references | Provide search/full-text access or source documents; keep unverified records provisional |
| Empty-state warnings | Expected for the blank template; fill real project details |
| CLI does not find the skill | Confirm the published repository is accessible and contains skills/create-research-paper/SKILL.md |
| Hosted validation has not run | Upload .github/workflows/validate.yml and enable repository Actions |

Installation references were checked on 2026-10-28: [Agent Skills specification](https://agentskills.io/specification), [OpenAI skill guide](https://learn.chatgpt.com/docs/build-skills), [Claude Code skills](https://code.claude.com/docs/en/skills), [Skills CLI reference](https://github.com/vercel-labs/skills/blob/main/docs/commands.md), and [GitHub setup-python](https://github.com/actions/setup-python). Product interfaces and third-party tools may change.
