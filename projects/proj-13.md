---
layout: post
title: 'rdc-cli'
---

Turn RenderDoc captures into Unix text streams. rdc-cli makes `.rdc` file contents accessible to `grep`, `awk`, `sort`, `diff`, `jq`, and AI agents.

### Features
- Explore captures like a filesystem: `rdc draws`, `rdc shader`, `rdc cat`
- Diff two captures: `rdc diff before.rdc after.rdc --draws`
- Remote capture from Linux/Windows hosts
- Full pipeline introspection for debugging GPU workloads

### Tech Stack
Python · RenderDoc API · Click · PyPI

[GitHub →](https://github.com/BANANASJIM/rdc-cli)
[Docs →](https://bananasjim.github.io/rdc-cli/)
