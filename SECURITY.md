# Security policy

## Supported versions

The SAP authentication fix is maintained in official
[`majd/ipatool`](https://github.com/majd/ipatool) version 2.4.0 and newer.
Users should migrate to the latest official release.

The final `2.3.2-sapfix.2` macOS transition build is compiled with Go 1.26.6.
The earlier `2.3.2-sapfix.1` binaries are historical and unsupported. The
`main` branch receives only security, repository-safety, documentation, and
build reproducibility updates; functional development belongs upstream.

## Reporting a vulnerability

Use [GitHub private vulnerability reporting](https://github.com/maksimryabkin/ipatool-sapfix/security/advisories/new)
for a vulnerability specific to this repository. For a vulnerability in current
official ipatool, follow the upstream project's reporting instructions.

Never put an Apple ID email address, password, two-factor code, DSID, session
token, cookie, authorization header, or raw authentication log in an issue or
advisory. A minimal redacted error and reproduction description are enough for
initial triage.
