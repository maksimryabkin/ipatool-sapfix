# ipatool-sapfix — final macOS App Store HTTP 403 fix

[![Status](https://img.shields.io/badge/status-maintenance-blue)](#project-status)
[![Release](https://img.shields.io/github/v/release/maksimryabkin/ipatool-sapfix?include_prereleases&label=transition%20release)](https://github.com/maksimryabkin/ipatool-sapfix/releases)
[![CI](https://github.com/maksimryabkin/ipatool-sapfix/actions/workflows/ci.yml/badge.svg)](https://github.com/maksimryabkin/ipatool-sapfix/actions/workflows/ci.yml)
[![CodeQL](https://github.com/maksimryabkin/ipatool-sapfix/actions/workflows/codeql.yml/badge.svg)](https://github.com/maksimryabkin/ipatool-sapfix/actions/workflows/codeql.yml)
[![License](https://img.shields.io/badge/license-MIT-yellow.svg)](LICENSE)

![ipatool-sapfix macOS App Store HTTP 403 fix and IPA downloader](resources/social-preview.png)

## Project status

> [!IMPORTANT]
> Official [`ipatool`](https://github.com/majd/ipatool) version 2.4.0 and newer
> now includes maintained SAP-signed App Store authentication. Functional
> development and current compatibility reports belong upstream.

This repository remains public as the historical macOS fix and as a searchable
answer for the error:

```text
request failed: unexpected response from Apple (HTTP 403): empty or non-plist body
```

The original patch added the required `X-Apple-ActionSignature` through macOS
CommerceKit and removed passwords and two-factor codes from verbose logs.
Upstream shipped a cross-platform SAP implementation in `v2.4.0` on August 28,
2026, followed by more authentication fixes in `v2.5.0`.

Official `v2.5.0` was verified here on September 1, 2026 with an existing
Keychain session and a live App Store version-list request. At that time,
however, Homebrew still packaged `v2.3.2`, while the official `v2.5.0` release
assets were compiled with the unpatched `go1.25.0` toolchain.

`2.3.2-sapfix.2` is therefore the final macOS transition release. It is rebuilt
with Go 1.26.6 and is published only after tests, vet, a live macOS SAP signer
smoke test, and `govulncheck` pass. Move to official ipatool once your package
source provides `v2.5.0` or newer built with a patched Go toolchain.

## Download the transition build

Download [`2.3.2-sapfix.2`](https://github.com/maksimryabkin/ipatool-sapfix/releases/tag/2.3.2-sapfix.2)
and choose the archive matching `uname -m`:

| Mac | `uname -m` | Archive |
| --- | --- | --- |
| Apple Silicon | `arm64` | `ipatool-2.3.2-sapfix.2-macos-arm64.tar.gz` |
| Intel | `x86_64` | `ipatool-2.3.2-sapfix.2-macos-amd64.tar.gz` |

Download the matching `.sha256sum` file as well. Apple Silicon example:

```shell
shasum -a 256 -c ipatool-2.3.2-sapfix.2-macos-arm64.tar.gz.sha256sum
tar -xzf ipatool-2.3.2-sapfix.2-macos-arm64.tar.gz
sudo install -m 0755 \
  bin/ipatool-2.3.2-sapfix.2-macos-arm64 \
  /usr/local/bin/ipatool
command -v ipatool
ipatool --version
```

For an Intel Mac, replace `arm64` with `amd64`. The binaries are not notarized.
If macOS blocks one, verify its checksum first and remove only that file's
quarantine attribute:

```shell
xattr -d com.apple.quarantine bin/ipatool-2.3.2-sapfix.2-macos-arm64
```

## Move to official ipatool

Check what Homebrew currently offers before upgrading:

```shell
brew update
brew info ipatool
```

Upgrade only when the displayed stable version is `2.5.0` or newer:

```shell
brew upgrade ipatool
ipatool --version
```

Until then, use the transition build above or build the official tag with a
patched Go toolchain:

```shell
git clone https://github.com/majd/ipatool.git
cd ipatool
git checkout v2.5.0
GOTOOLCHAIN=go1.26.6 go test ./...
GOTOOLCHAIN=go1.26.6 go build -trimpath \
  -ldflags="-X github.com/majd/ipatool/v2/cmd.version=2.5.0-local" \
  -o ipatool .
sudo install -m 0755 ipatool /usr/local/bin/ipatool
```

## Authenticate

Check the current session before logging in again:

```shell
ipatool auth info
```

Only if no account is shown, authenticate interactively so the password is not
stored in shell history:

```shell
ipatool auth login --email "you@example.com"
```

## Security and support

Never publish raw authentication logs. Redact email addresses, passwords,
two-factor codes, tokens, cookies, authorization headers, and DSIDs.

Report current ipatool behavior and feature requests in
[upstream Issues](https://github.com/majd/ipatool/issues). Use this repository's
Issues only for a security, build, or documentation problem specific to this
fork. See the [security policy](SECURITY.md) for private reports.

The repository and its release artifacts contain no Apple account credentials
or authentication state. Local authentication state remains in the user's
credential store and is not part of Git.

## Build the historical source

Use Go 1.26.6 or newer. On macOS with Xcode command line tools installed:

```shell
git clone https://github.com/maksimryabkin/ipatool-sapfix.git
cd ipatool-sapfix
go generate ./...
go test ./...
go vet ./...
go build ./...
```

GitHub Actions also runs the macOS SAP signer smoke test, `govulncheck`, and
CodeQL. Release automation is macOS-only, uses pinned official actions and
minimal permissions, and cannot write to an unrelated Homebrew repository.

## Credits and license

Based on [`majd/ipatool`](https://github.com/majd/ipatool) and distributed under
the [MIT License](LICENSE). The original copyright and license notice and full
Git history are preserved.
