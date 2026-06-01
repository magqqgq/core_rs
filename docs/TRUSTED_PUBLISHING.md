# Trusted publishing

Releases are published to [crates.io](https://crates.io/crates/core_rs) from GitHub Actions using [trusted publishing](https://crates.io/docs/trusted-publishing).

## One-time crates.io setup

In the crate settings on crates.io, add a **Trusted Publisher**:

| Field | Value |
|-------|--------|
| Provider | GitHub Actions |
| Owner | `coinbase` |
| Repository | `core_rs` |
| Workflow file | `publish.yml` |

Ensure `.github/workflows/publish.yml` exists on the `master` branch before saving.

After the first successful trusted publish, you may enable **Trusted Publishing Only** to disable long-lived API tokens.

## Release process

1. Bump the version in `Cargo.toml` and update `CHANGELOG.md`.
2. Merge to `master` on [coinbase/core_rs](https://github.com/coinbase/core_rs).
3. Create and push a version tag (for example `v0.2.0`):

   ```sh
   git tag v0.2.0
   git push origin v0.2.0
   ```

4. The [Publish](https://github.com/coinbase/core_rs/actions/workflows/publish.yml) workflow runs `cargo publish --locked` using a short-lived OIDC token.

To retry without a new tag, use **workflow_dispatch** on the Publish workflow.
