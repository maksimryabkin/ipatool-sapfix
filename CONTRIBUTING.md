# Contributing

This repository preserves the macOS compatibility fix that restored App Store
login in August 2026. The maintained official project now includes SAP-signed
authentication, so functional changes and feature requests should go to
[`majd/ipatool`](https://github.com/majd/ipatool).

Small corrections specific to this historical repository remain welcome,
especially security, documentation, and build reproducibility fixes. Before
opening a pull request, run:

```shell
go generate ./...
go test ./...
go vet ./...
go build ./...
```

Do not include credentials, tokens, cookies, raw authentication logs, generated
IPA files, or machine-specific paths in commits or pull requests.
