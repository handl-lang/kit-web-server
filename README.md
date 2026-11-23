# kit-web-server

Serve HTTP from handl — a `kit.hnl` v1 curation manifest over `lib-*`
packages (proposal P-004): the epoll accept loop (`serve-epoll` over
`concurrent-epoll`), the HTTP/1.1 wire codec (`http-rfc`), TLS
(`tls-libssl`), WebSocket (`ws-*`), and JSON/string/bytes utilities.

## Manifest

`kit.hnl` is a pure curation manifest — `{ name, description, license,
libs }` only. No plugins, no entry/glue (kit.hnl v1, user-approved
2026-11-22).

**`libs` convention:** each entry is a lib *package* name — the `lib-*`
GitHub repo name minus the `lib-` prefix (e.g. `lib-serve-epoll` →
`"serve-epoll"`, `lib-comb` → `"comb"`). Resolution goes through each
package's own `pkg.hnl`: its `modules` declare what it ships and its
`deps` pull contracts and transitive backends, so the kit lists the
curated set, not the closure. (`lib-curl`/`lib-libc` ship metadata-only
manifests — codegen FFI bindings consumed via path deps; `lib-bytes` is a
Rust-plugin module with no `pkg.hnl` yet.)

Serving needs no glue: `serve((req) ~> resp, {port: 8080, tls?, on_ws?})`.
Routing lives in `http-route` (named route maps, `:params`, `*wildcards`,
405s) and `http-middleware` adds the onion-style `(ctx, next)` pipeline
(CORS, logger, request-id, session, auth, CSRF, body-limit); both compose
around the handler arrow. Linux-only (epoll/libcurl/libssl).

## Layout

```text
kit.hnl   the curation manifest (the whole repo)
```

## Gate

None yet — no `hnl` binary is available to validate `kit.hnl` parses.
When the kit.hnl loader lands (host-side or pkg), a parse/lint gate can
be added (`scripts/hnl-gate.sh` pattern from the `lib-*` repos).

## License

MIT.
