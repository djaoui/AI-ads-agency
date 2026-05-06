# AI-ads-agency

Portable toolkit for producing AI-generated paid social and brand ads at agency quality. Survives device loss; deploys onto any new Claude Code install with one command.

## What's in here

```
/skills/ai-ads-production/SKILL.md  ← Master SOP (two-pass workflow, hard rules, format fingerprints, pricing, failure-mode index)
/agents/                            ← Reusable agent prompt templates
  creative-director-agent.md        ← Brand-anchored prompt writer + QA filter
  mechanical-qa-agent.md            ← Frame-by-frame visual defect detector
  bible-builder-agent.md            ← Deduces brand bible from public artefacts
/brand-bibles/                      ← One file per client (lived editorial source of truth)
  _template.md
/sops/                              ← Sub-SOPs referenced from the master skill
/failures-log/                      ← Verified failures + fixes (knowledge accretion)
```

## Deploy on any machine

```bash
git clone https://github.com/djaoui/AI-ads-agency.git ~/.claude/skills/AI-ads-agency
# or symlink an existing clone:
# ln -s ~/path/to/AI-ads-agency ~/.claude/skills/AI-ads-agency
```

After clone, restart Claude Code. The skill loads on every session.

## Pull updates from any machine

```bash
cd ~/.claude/skills/AI-ads-agency && git pull
```

## How to use in Claude Code

When starting a new ad project:
1. Identify client → load (or build) their `/brand-bibles/[client].md`
2. Use Creative Director agent to write the prompt(s), Bible attached
3. Generate
4. Run Mechanical QA agent on the output
5. Creative Director agent filters QA findings through the Bible — only ship-blockers surface
6. Iterate

## License
Private — internal use.
