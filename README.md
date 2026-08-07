# @dreki-gg/pi-handoff

Transfer context to a new focused [pi](https://github.com/earendil-works/pi-coding-agent) session instead of lossy compaction.

`/handoff` extracts the important context for your next task. It generates an editable prompt and opens a new session with parent tracking.

The prompt includes a local link to the previous session JSONL file. The next agent can use `read` or `rg` on this file if the summary omits a detail. Ephemeral sessions do not include this link.

## Usage

```
/handoff now implement this for teams as well
/handoff execute phase one of the plan
/handoff check other places that need this fix
```

You must use interactive mode and select a model.
