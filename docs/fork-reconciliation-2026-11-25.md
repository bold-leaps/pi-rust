# Fork reconciliation: upstream 0.28.0 (2026-11-25)

## Status

- **Upstream synchronized:** `main` was fast-forwarded from `aab74de0737f4b7c454cd8c8c16ca66e4dfbf898` to upstream `946c5b6d9c2ea7575b74f2a00dcd584aabf9cebc`.
- **Custom features:** not yet reimplemented or validated against the new upstream architecture.
- **Safety:** original work is retained in its existing feature branches and explicit backup refs; do **not** force-push or delete those refs.
- **Testing:** no local Cargo build or test run was possible in the current tool environment. This is a migration inventory, not a claim of parity.

## Preserved refs

| Purpose | Branch | Preserved commit |
| --- | --- | --- |
| Former fork main | `backup/main-before-upstream-2026-11-25` | `aab74de0737f4b7c454cd8c8c16ca66e4dfbf898` |
| Main fork-specific feature history | `backup/extension-generations-2026-10-26` | `278cde7d20db1a5424153633b1808bc8c3928655` |
| Independent reusable model selectors | `backup/reusable-selectors-2026-10-20` | `cec852fbc437c632899e7966313dabc9ee460eb3` |

The extended branch has **118 commits ahead of the former fork main**, including merged upstream work; do not count all 118 as unique fork patches. The original feature branches remain accessible.

## Reapplication workstreams

These are **candidates**, not a verified list of missing upstream features. Compare current behavior and API contracts with the original change before porting anything.

1. **Provider and model host APIs.** Earlier work extracted `rpi-model-runtime`, safe model execution provenance, provider catalogs and configurations, protected credentials, provider lifecycle, and atomic refresh. The current upstream workspace instead has `rpi-runtime` and `rpi-durable`. Adapt the public behavior to upstream runtime ownership, rather than restoring the obsolete crate tree or changing workspace versions backward.
2. **Provider request identity and attestation.** Recheck safe header/body identity, provider-option attestation, endpoint handling, continuation safety, and redaction. The prior work includes `crates/rpi-ai/src/request_effect_identity.rs` and `provider_option_attestation.rs`. Ensure secret material never appears in logs, fingerprints, or UI; add collision and configuration-change tests.
3. **Extension isolation and generations.** Reconcile the worker protocol, isolated provider transport, host capability allowlists, cancellation, publication supersession, and session-generation lifecycle against the newer extension and durable-session APIs. Prefer secure bounded behavior, with tests for leaked capabilities, stale generations, and worker shutdown.
4. **Reusable TUI and selector APIs.** The separate `codex/reusable-agent-selectors` commit introduced `rpi-agent-ui` and provider/model/thinking selector components. Check the current upstream `rpi-tui` and CLI selector APIs before adapting. Reuse native selectors, preserve provider-qualified model identity, and avoid bringing back obsolete `Model` or `ThinkingLevel` assumptions.
5. **Terminal behavior and controls.** Reassess Windows ConPTY press/release and Enter handling, shutdown input, recording raw-mode restoration, overlay padding, focus dispatch, tree-list viewport bounds, and host UI lifecycle. Only reinstate fixes where modern upstream tests reveal the regression.
6. **ScribeRig interoperability.** Identify the original integration requirements and API seams first; upstream history and architecture may already satisfy some. Port only the missing public capabilities.

## Integration criteria

For each workstream:

- Identify source commits and enumerate user-visible, host API, security, and test requirements.
- Search upstream `main` for equivalent functionality before creating a patch.
- Port the *behavior* as a small scoped change based on current upstream, not a bulk cherry-pick of all historical commits.
- Keep compatibility with the current workspace manifest and minimum supported Rust version.
- Add targeted regression tests; run `cargo fmt --all -- --check`, `cargo check --workspace --all-targets`, `cargo test --workspace`, and available CI. Document any failures and platform-specific checks.
- Keep each coherent feature in its own reviewable pull request.
- Do not modify release artifacts, version numbers, or public APIs merely to match the old fork.

## References

- [Previous fork development branch](https://github.com/bold-leaps/pi-rust/tree/backup/extension-generations-2026-10-26)
- [Independent model selectors](https://github.com/bold-leaps/pi-rust/tree/backup/reusable-selectors-2026-10-20)
- [Upstream repository](https://github.com/bigfish1913/pi-rust)
- [Synchronized fork main](https://github.com/bold-leaps/pi-rust/tree/main)
