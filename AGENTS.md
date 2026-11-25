# kit-web-server

Serve HTTP from handl — a `kit.hnl` v1 curation manifest over `lib-*`
packages (proposal P-004): the epoll accept loop (`serve-epoll` over
`concurrent-epoll`), the HTTP/1.1 wire codec (`http-rfc`), TLS
(`tls-libssl`), WebSocket (`ws-*`), and JSON/string/bytes utilities.

## Layout

| path | role |
| --- | --- |
| `kit.hnl` | the kit.hnl v1 curation manifest — the whole repo |
| `workflows/` | bupha gate wiring |

## Contract

- `kit.hnl` is pure curation — `{ name, description, license, libs }`
  only; no plugins, no entry/glue (kit.hnl v1, user-approved
  2026-11-22).
- **`libs` convention:** each entry is a lib *package* name — the
  `lib-*` repo name minus the `lib-` prefix. Resolution goes through
  each package's own `pkg.hnl`: `modules` declare what it ships, `deps`
  pull contracts and transitive backends — the kit lists the curated
  set, not the closure.
- Serving needs no glue: `serve((req) ~> resp, {port: 8080, tls?,
  on_ws?})`. Routing lives in `http-route`; `http-middleware` adds the
  onion-style `(ctx, next)` pipeline. Linux-only (epoll/libcurl/libssl).

## Verification

Manifest-only repo — no build.

<!-- myco:agent-awareness -->

## Agent awareness

- Session start / any work discussion: run `myco status --prompt`
  before proposing next steps — know the board first.
- User describes new work → offer to capture it (`myco todo add`,
  `issue new`, `proposal new`). Don't wait to be asked.
- Picking up a task: `myco resume <ID>` is the briefing.
- Wrapping up: `myco handoff <ID>` anything resumable; keep statuses
  honest (claimed → in-progress → blocked/done).

<!-- /myco:agent-awareness -->
