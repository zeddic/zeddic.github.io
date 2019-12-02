---
layout: page
title: Misc bash snippits
permalink: /notes/msic_bash
---

This is a collection of misc bash snippits for my own memory.

```
# Converting all files in a directory to unix EOL markers
find . -type f -print0 -name '*.ext' | xargs -0 dos2unix
```