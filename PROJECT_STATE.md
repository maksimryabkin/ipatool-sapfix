# Project state

Updated: 2026-09-01 (Europe/Moscow)

## Identity

- Project: `ipatool-sapfix`, the historical macOS App Store authentication
  compatibility fork.
- Canonical local root: `/Users/maksim/Projects/active/ipatool-sapfix`.
- Public repository: `https://github.com/maksimryabkin/ipatool-sapfix`.
- Branch: `main`.
- Maintenance commit and release source: `ca83e408a30b624b806cbb9c7ca8130d2eef503d`.
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
- GitHub CI run `33520274417`: passed on macOS and Linux, including the SAP
  smoke test and vulnerability scan.
- GitHub CodeQL run `33520274639`: passed; open code-scanning alerts: zero.
- Actions are restricted to GitHub-owned actions and immutable SHA references.
  Workflow tokens default to read-only.
- Dependabot alerts, security updates, secret scanning, push protection, and
  private vulnerability reporting are enabled.

## Community state

- All repository issues are answered and closed. Issue #1 records the final
  transient-authentication result and migration path. Issue #4 has a corrective
  note that current official ipatool supports Windows.
- Current reports should be filed at `https://github.com/majd/ipatool/issues`.
  This repository accepts only security, build, or documentation reports that
  are specific to the historical fork.
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
