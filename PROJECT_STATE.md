# Project state

Updated: 2026-09-01 (Europe/Moscow)

## Identity

- Project: `ipatool-sapfix`, the historical macOS App Store authentication
  compatibility fork.
- Canonical local root: `/Users/maksim/Projects/active/ipatool-sapfix`.
- Public repository: `https://github.com/maksimryabkin/ipatool-sapfix`.
- Branch: `main`.
- Maintenance commit and release source: `ca83e408a30b624b806cbb9c7ca8130d2eef503d`.
- Verified repository tip before this handoff-only checkpoint:
  `4ffad68a9cdd40ecd0b9808cb20240864b2fdc87`.
- Final transition tag and prerelease: `2.3.2-sapfix.2`.
- Upstream reference checked: `majd/ipatool` `v2.5.0` at
  `d5d0b56faf64e3fdef885d49e7928b390aadb6c7`.

## Product status

- Official ipatool `v2.4.0` and newer now contains maintained SAP-signed
  authentication. Functional development and current compatibility reports
  belong upstream.
- Official `v2.5.0` was verified on this Apple Silicon Mac on 2026-09-01: the
  existing Keychain account was recognized and a live authenticated
  `list-versions` request succeeded.
- As checked on 2026-09-01, Homebrew stable still packaged `v2.3.2`, and the
  upstream `v2.5.0` release asset was compiled with `go1.25.0`. Building the
  same upstream source with `go1.26.6` passed generate, tests, vet, build, and
  source `govulncheck` with zero reachable vulnerabilities.
- This repository remains public for search/provenance and supplies one final
  macOS transition build. Future functional changes should not be developed
  here.

## Release verification

- GitHub Actions release run `33520513797` completed successfully.
- Both published archives were downloaded again and their `.sha256sum` files
  passed `shasum -a 256 -c`.
- The extracted binaries are native Mach-O `arm64` and `x86_64` executables.
- Both binaries report `ipatool version 2.3.2-sapfix.2` and Go `1.26.6`.
- The published ARM binary recognized the existing account and completed a
  live authenticated `list-versions` request on 2026-09-01.
- Source-mode `govulncheck` reports zero reachable vulnerabilities. Binary mode
  conservatively reports three symbol families from transitive `x/crypto` and
  `x/net` code; the vulnerable calls are absent from the source call graph (and
  the reported OpenPGP/server-SSH packages are not source dependencies). Do not
  describe the binary scan itself as warning-free.
- Local `/opt/homebrew/bin/ipatool` points to
  `/Users/maksim/.local/share/ipatool/2.3.2-sapfix.2/ipatool`. The previous
  `.1` binary remains available locally for rollback.
- The Intel binary's Go VCS metadata reports `modified=true` because the first
  architecture was packaged inside the checkout before the Intel build. The
  source verification ran before packaging and the published checksum is
  correct. The workflow now builds outside the checkout so this cannot recur;
  the immutable `.2` asset was not replaced for a cosmetic metadata flag.

## Verification and automation

- Local `go generate ./...`, `go test ./...`, `go vet ./...`, and
  `go build ./...`: passed with Go 1.26.6 on 2026-09-01.
- Local CommerceKit integration test: passed.
- Local source `govulncheck@v1.7.0 ./...`: zero reachable vulnerabilities.
- Cross-builds for macOS `arm64` and `amd64`: passed.
- Final GitHub CI run `33522878310`: passed on macOS and Linux, including the
  SAP smoke test and vulnerability scan.
- Final GitHub CodeQL run `33522878380`: passed; open code-scanning alerts:
  zero.
- Actions are restricted to GitHub-owned actions and immutable SHA references.
  Workflow tokens default to read-only.
- Dependabot alerts, security updates, secret scanning, push protection, and
  private vulnerability reporting are enabled.
- `main` requires the macOS test, Linux test, vulnerability scan, and CodeQL;
  strict checks, linear history, and resolved conversations are enforced.
  Force-pushes and branch deletion are disabled. Admin enforcement remains off
  so the solo owner retains an emergency bypass.

## Community state

- All repository issues are answered and closed. Issue #1 records the final
  transient-authentication result and migration path. Issue #4 has a corrective
  note that current official ipatool supports Windows.
- Current reports should be filed at `https://github.com/majd/ipatool/issues`.
  This repository accepts only security, build, or documentation reports that
  are specific to the historical fork.
- Dependabot PR #11 is the one allowed grouped monthly maintenance update. Its
  checks pass, but it upgrades ten dependencies and is intentionally not
  auto-merged into the already released transition build.
- A downstream repository copied the Git history and continued independently.
  It is not a GitHub-native fork, but it preserves the MIT license and history.

## Security rules

- Never commit, publish, or summarize Apple account identities, passwords,
  two-factor codes, DSIDs, cookies, session tokens, authorization headers, or
  raw authentication logs.
- Local authentication state belongs to macOS Keychain and is not part of the
  repository, release assets, or project state.
- The old `2.3.2-sapfix.1` assets are retained for provenance but are marked
  unsupported and superseded. Do not delete or replace immutable release
  assets.

## Maintenance policy

1. Keep security automation and the exact HTTP 403 text available for users
   arriving from search.
2. Group routine dependency updates monthly; merge only after all required
   checks pass and the change is relevant to maintenance.
3. Do not publish another feature release. If a severe fork-specific security
   issue requires a binary update, document the reason and create a new tag;
   never overwrite `.2`.
4. Re-check Homebrew/upstream packaging before changing migration instructions.

The unrelated BotHelp/MAX attribution task that once used the old ipatool
working directory is not part of this product and must not be merged into its
source or continuity.

## Rotation handoff

- Objective: keep the final public SAP/HTTP 403 transition repository usable,
  searchable, secure, and accurately directed toward maintained upstream
  ipatool.
- Completed: published and re-verified `2.3.2-sapfix.2`, updated the local
  installation, answered and closed all current issues, hardened GitHub
  Actions and repository security settings, and documented the upstream
  migration path.
- Git state before this documentation-only checkpoint: branch `main`, commit
  `4ffad68a9cdd40ecd0b9808cb20240864b2fdc87`; the working tree had no modified,
  untracked, or unpushed files and `origin/main` resolved to the same commit.
  The commit containing this paragraph is a handoff-only successor.
- Deployment state: the public GitHub repository and immutable `.2` release are
  live. There is no server deployment. Locally, `/opt/homebrew/bin/ipatool`
  resolves to the verified `.2` ARM binary; `.1` remains available for local
  rollback.
- Final checks: `go generate ./...`, `go test ./...`, `go vet ./...`,
  `go build ./...`, source `govulncheck`, macOS/Ubuntu CI, CodeQL, archive
  checksums, binary architecture/version checks, and the authenticated macOS
  SAP smoke request all passed.
- Next step: this maintenance stage is complete. A continuation task should
  read this file, confirm the repository remains clean, and wait for the user's
  next request. Do not invent further changes. Triage a new issue or security
  alert only when one actually appears. Grouped Dependabot PR #11 remains open
  intentionally for a deliberate future dependency review.
- Remaining risks: Homebrew stable was still the unfixed `2.3.2` package on
  2026-09-01; re-check before changing install guidance. Binary-mode
  `govulncheck` has the conservative transitive-symbol limitation documented
  above. The Intel archive has the cosmetic VCS `modified=true` metadata noted
  above. Never overwrite the immutable `.2` release or expose local Keychain
  authentication state.
