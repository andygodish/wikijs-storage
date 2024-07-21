---
title: Global Replace
description: Global replacement using vi/vim text editor. 
published: true
date: 2024-07-21T15:52:53.502Z
tags: vi, unix, vim, text
editor: markdown
dateCreated: 2024-07-21T15:52:53.502Z
---

# Global Replace

Scenario: Use `vi` (Visual Interface) to globally replace all instances of the string pattern, `1.4.0` with `1.4.1`.

From within the vi text editor,
```
:%s/1.4.0/1.4.1/gci
```

Where,

- `:%` - Apply the command to the entire file.
- `s` - Substitute command.
- `1.4.0` - The string you want to replace.
- `1.4.1` - The string you want to replace it with.
- `g` - Replace all occurrences in each line (global).
- `c` - Confirm each replacement.
- `i` - Ignore case in the search pattern.

Simply cycle through each instance to individually update. 