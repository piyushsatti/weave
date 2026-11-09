# weave

A compiler for agent plugins, frozen at v0.1.2. Write a plugin once and weave writes one finished
folder per coding-agent harness. It stopped growing on October 22, 2026, and this README says why.

## What works today

- One manifest in, one folder out for each of six targets: Claude Code, Codex, OpenCode, Pi, the
  portable Agent Plugins package that Cursor, GitHub Copilot and VS Code read, and a set of
  repository instruction files.
- A target that cannot carry something refuses to build. The error names the item, the manifest
  line and both ways forward, and it starts `CANNOT SHIP THIS TO PI.` when the target is Pi. You
  either drop the target or write the loss down under `degrade`, and a written-down loss is
  printed on every build and recorded in each folder's `foundry.lock.json` and in
  `foundry.release.json`.
- Nothing is built or fetched on the user's machine. The build runs in the plugin's own CI, and
  people install the folder for their harness as it arrived.
- Dependencies on other plugins are pinned by fingerprint, never by a moving name like `main`.
- The repository has a test suite and CI, and `v0.1.2` is tagged.

## The bottleneck

Most of a plugin is the same everywhere: a skill is a folder with a `SKILL.md`, and MCP config is
close to identical. A compiler adds little for those. Hooks are where harnesses differ, each with
its own events and input shapes, and they have to be written by hand per harness whatever the
tool does. weave defines six neutral hook moments, but of its six targets only the Claude Code back
end carries hooks. The compiler generated the parts that were already the same.

## Status

Frozen. No new targets or features, and plugins built with weave keep working from their last
release. I moved my own plugins to one folder per plugin that Claude Code, Codex and Cursor read
directly, plus a lint script. The write-up is
[Why I froze my own plugin compiler](https://piyushsatti.com/blog/why-i-froze-my-plugin-compiler/).

## Try it

Copy `template/` rather than using GitHub's "Use this template" button, which copies the
repository root and misses the manifest.

```
cp -R template/ ../my-plugin
cd ../my-plugin
git init
python3 scripts/foundry.py check
```

Requires Python 3 and git. `check` builds into a temporary directory and throws it away.
`python3 scripts/foundry.py build --out dist` writes the folders you would publish.

## Naming

This project was called foundry, and the code still uses that name in `foundry.plugin.yaml`,
`foundry.lock.json` and `scripts/foundry.py`. They are left as they are because renaming them
would break plugin repositories already built with it. The bootstrap stub fetches from
`github.com/piyushsatti/weave.git`.

MIT, see `LICENSE`.
