# cc-skills-golang — tao-io fork

A **maintained fork** of [samber/cc-skills-golang](https://github.com/samber/cc-skills-golang)
repackaged as a single Claude Code meta-skill named `golang`. The
discoverable entry point is the root `SKILL.md`; every sub-skill from
the upstream pack lives in its own directory and is read **on demand**
by the dispatcher.

## What changed vs upstream

| Aspect | Upstream | This fork |
|---|---|---|
| Layout | `skills/golang-<topic>/SKILL.md` | `<topic>/sub-SKILL.md` (flattened, prefix dropped) |
| Inner manifest filename | `SKILL.md` | `sub-SKILL.md` (opts out of nested skill auto-discovery) |
| Dispatcher | none (each skill is independent) | root `SKILL.md` routes Reads to the right sub-skill |
| Marketplace metadata | `.claude-plugin/`, `.cursor-plugin/`, `gemini-extension.json`, `clawhub-publish.sh` | removed (not used in this internal layout) |
| Upstream `CLAUDE.md` (skill authoring guide) | present | removed (not consumed locally) |
| Sub-skill bodies, `references/`, `evals/`, `assets/` | preserved verbatim | preserved verbatim |
| Cross-references | `samber/cc-skills-golang@golang-<topic>` | rewritten to relative `<topic>/sub-SKILL.md` paths |

The structural changes exist so that Claude Code's auto-discovery
surfaces exactly **one** skill (`golang`) in the global chooser instead
of 43, while the dispatcher loads detailed sub-skill content lazily.

## Install locally

```bash
cd ~/.claude/skills
git clone https://github.com/tao-io/cc-skills-golang.git golang
```

After install, Claude Code sees a single `golang` skill. When a Go task
matches a sub-skill's triggers (see Dispatch in `SKILL.md`), the
dispatcher Reads the relevant `<topic>/sub-SKILL.md`.

## Sync from upstream

This fork has diverged structurally — you cannot fast-forward merge
upstream commits. Cherry-pick instead:

```bash
git fetch upstream
git log upstream/main --oneline -20             # find a commit of interest
git cherry-pick <commit>                        # resolve conflicts manually
```

For broad content refresh (e.g. upstream rewrote a sub-skill), the
quickest path is usually:

```bash
git fetch upstream
git checkout upstream/main -- skills/golang-<topic>/
git mv skills/golang-<topic> <topic>
git mv <topic>/SKILL.md <topic>/sub-SKILL.md
# then re-run the sed cross-ref rewrite (see commit history under
# feat/tao-flatten-dispatcher for the exact pattern)
rm -rf skills
git add -A && git commit -m "chore: sync <topic> from upstream"
```

## Attribution

Original work © [@samber](https://github.com/samber), MIT licensed.
The upstream repository contains the authoritative methodology
(`GOLANG-AI-DRIVEN-REVIEW.md`) and evaluation uplifts
(`EVALUATIONS.md`), both preserved in this fork for reference.
