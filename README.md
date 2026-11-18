# SEAMS runtime - launcher + your node

The Seams demo runtime in one box: the launcher UI and your node -
a reference node you run locally - both loopback-only, zero-dependency Node (stdlib only). The wire
spec, the 145-case vector suite and the battle-test selftest live in the
sibling protocol repo (working name `seams-protocol`) - not here.

**Working repo name `seams-runtime` - final naming is the owner's call
before any push.**

## Quickstart

Prereq: Node 18+ (no `npm install` needed on the runtime path).

```sh
git clone <this repo> && cd seams-runtime
sha256sum -c MANIFEST.sha256          # verify every shipped file
node eom/launcher/launch.mjs
```

- Launcher UI: `http://127.0.0.1:4173`
- your node: `http://127.0.0.1:8080` (spawned by the launcher)

Ctrl-C stops both servers the launcher started (it supervises only what it
spawns; a node you started yourself stays yours). Env knobs, all
optional: `PORT`, `NODE_ONE_PORT`, `NODE_ONE_URL` (expert override - reuse
an already-running loopback node), `OPS_DIR`, `NODE_ONE_DATA_DIR`. The
launcher binds 127.0.0.1 only and never dials outward.

## What is in the box

| Path | What it is |
|---|---|
| `eom/launcher/launch.mjs` | One command, both servers, loopback only |
| `eom/launcher/public/` | The launcher UI - plain HTML/CSS/JS served by `server.mjs` |
| `eom/launcher/design-handoff-2026-10-26/` | The pinned design handoff, kept verbatim (`CHECKSUMS.sha256`) |
| `eom/node-one/attributed-text/` | Your node (a reference node you run locally) - one page of text anyone can change; every word keeps the name of who wrote it; browser-minted keys, signed deposits |
| `eom/endpoint-surface/` | The join-gate contract: `requirements.yaml` REQ tables, key custody provenance, Cloudflare template + fingerprint verify |
| `tests/` | Endpoint-surface conformance: spawns your node itself, loopback-only, asserts the REQ tables at the wire level |
| `MANIFEST.sha256` | sha256 of every shipped file |
| `PROVENANCE.txt` | Lineage of every shipped artifact |
| `LICENSE` | The seed license, shipped verbatim |

## Tests

```sh
cd eom/node-one/attributed-text && node --test test/*.test.mjs
cd eom/launcher && node --test test/launcher.test.mjs
python3 -m pytest tests/   # conformance; spawns your node on an ephemeral loopback port
```

## The other half of the split

The protocol repo (working name `seams-protocol`) holds the wire spec
(`spec/WIRE-V0.md`), the nine canonical invariants, the counted vector suite
(145 cases) and the battle-test kit. Clone it and verify with:

```sh
cd kit && python3 battle.py selftest   # expect: stub: 145/145 vectors pass
```

Its README carries the reading order.
