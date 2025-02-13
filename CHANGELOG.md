caa
### Fixed
* Fix shutdown handling to check all errors
* Fix nil pointer dereference of shutdown without s.Echo

## 25.01.2 - 2025-01-17
### Changed
* Refactor migration application logic to apply each missing migration individually and to print infos about the migration being applied
* Refactor the cobra cmd setup structure and the flag parsing logic using struct-based flags.
**BREAKING**: The restructuring breaks additionally created subcommands attached to the existing commands. See [Migration Guide Command Restructuring](https://github.com/allaboutapps/go-starter/wiki/Migration-Guide-Command-Restructuring)


## 25.01.1 - 2025-01-03
### Changed
* Bump [github.com/volatiletech/sqlboiler/v4 from v4.17.1 to v4.18.0](https://github.com/volatiletech/sqlboiler/releases/tag/v4.18.0)

## 25.01.0 - 2025-01-02
### Added
* Migrate the changelog tracking from manual tracking to the changelog tracking tool [changie](https://github.com/miniscruff/changie)
### Changed
* Update to [golang:1.23.4-bookworm](https://hub.docker.com/layers/library/golang/1.23.4-bookworm/images/sha256-5c3223fcb23efeccf495739c9fd9bbfe76cee51caea90591860395057eab3113) (requires `./docker-helper.sh --rebuild`)
* [Bump github.com/golangci/golangci-lint from v1.59.0 to v1.62.2](https://github.com/golangci/golangci-lint/releases/tag/v1.62.2)
* `go.mod` updates:
  - Minor: [Bump github.com/BurntSushi/toml from v1.3.2 to v1.4.0](https://github.com/BurntSushi/toml/releases/tag/v1.4.0)
  - Patch: [Bump github.com/gabriel-vasile/mimetype from v1.4.3 to v1.4.7](https://github.com/gabriel-vasile/mimetype/releases/tag/v1.4.7)
  - Minor: [Bump github.com/go-openapi/errors from v0.21.0 to v0.22.0](https://github.com/go-openapi/errors/releases/tag/v0.22.0)