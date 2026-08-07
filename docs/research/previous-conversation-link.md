# Previous conversation links in pi handoffs

## Decision

Use `ctx.sessionManager.getSessionFile()` as the transcript reference. It returns the current JSONL path or `undefined` for an ephemeral session.

Show the plain path in the prompt. Add a `file://` target for terminals that support links.

Agents can pass the plain path to `read`, `grep`, or `rg`. Use `pathToFileURL()` to make a valid link target.

Sources:

- `/Users/jalbarran/.bun/install/global/node_modules/@earendil-works/pi-coding-agent/dist/core/session-manager.d.ts` — `ReadonlySessionManager.getSessionFile()`
- `/Users/jalbarran/.bun/install/global/node_modules/@earendil-works/pi-coding-agent/dist/core/tools/read.js` — `createReadToolDefinition()`
- `/Users/jalbarran/.bun/install/global/node_modules/@earendil-works/pi-coding-agent/dist/core/tools/grep.js` — `createGrepToolDefinition()`
- `/Users/jalbarran/.bun/install/global/node_modules/@earendil-works/pi-coding-agent/dist/core/tools/path-utils.js` — `resolveToCwd()`

## Compaction behavior

Pi appends each compaction entry to the current JSONL file. It does not remove the earlier entries or change the file path.

Compaction is lossless for the stored transcript. It is lossy only for the context that Pi sends to the model.

A raw search can match inactive branches because the JSONL file stores all session branches. Use `getBranch()` when exact branch data is necessary.

Sources:

- `/Users/jalbarran/.bun/install/global/node_modules/@earendil-works/pi-coding-agent/docs/compaction.md` — “How It Works”
- `/Users/jalbarran/.bun/install/global/node_modules/@earendil-works/pi-coding-agent/docs/session-format.md` — “Tree Structure” and “Context Building”
- `/Users/jalbarran/.bun/install/global/node_modules/@earendil-works/pi-coding-agent/dist/core/session-manager.js` — `appendCompaction()`, `getBranch()`, and `buildContextEntries()`

## Session replacement

Capture the path before `ctx.newSession()`. The replacement session has a different current transcript.

Pass the captured path as `parentSession`. Pi also stores this value in the replacement session header.

The replacement session sets `PI_SESSION_FILE` to its own transcript. Thus, the handoff prompt must carry the previous path explicitly.

Sources:

- `/Users/jalbarran/.bun/install/global/node_modules/@earendil-works/pi-coding-agent/docs/extensions.md` — `ctx.newSession(options?)`
- `/Users/jalbarran/.bun/install/global/node_modules/@earendil-works/pi-coding-agent/docs/session-format.md` — `SessionHeader`
- `/Users/jalbarran/.bun/install/global/node_modules/@earendil-works/pi-coding-agent/docs/environment-variables.md` — “Bash Tool Session Environment”

## Limits

An ephemeral session has no transcript path. In this case, keep the generated summary and omit the link.

A local path does not work on a different host unless that host mounts the same session storage.

The transcript can contain messages, reasoning, tool calls, and tool results. The editable handoff draft lets the user remove the link before submission.

Sources:

- `/Users/jalbarran/.bun/install/global/node_modules/@earendil-works/pi-coding-agent/docs/session-format.md` — “Message Types” and “Entry Types”
- `/Users/jalbarran/.bun/install/global/node_modules/@earendil-works/pi-coding-agent/docs/security.md` — “No Built-in Sandbox”
- `/Users/jalbarran/.bun/install/global/node_modules/@earendil-works/pi-coding-agent/docs/usage.md` — “Exporting and Sharing Sessions”
