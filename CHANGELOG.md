
# Changelog

- All notable changes to this project will be documented in this file.
- The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
- We **do not follow [semantic versioning](https://semver.org/)**.
- There are **no git tags**. 
- All changes are solely **tracked by date**. 
- The latest `master` is considered **stable** and should be periodically merged into our customer projects.

## Unreleased
### Changed
- **BREAKING** Switched from [`golint`](https://github.com/golang/lint) to [`revive`](https://github.com/mgechev/revive)
  - ['golint' is deprecated](https://github.com/golang/go/issues/38968).
  - `revive` is considered to be a drop-in replacement for `golint`, however this change still might lead to breaking changes in your codebase.

## 2021-05-18
### Changed
- Upgraded `Dockerfile` to `golang:1.16.4`, `gotestsum@v1.6.4`, `golangci-lint@v1.40.1`, `watchexec@v1.16.0` (requires `./docker-helper.sh --rebuild`).
- Upgraded `go.mod`:
  - [github.com/labstack/echo/v4@v4.3.0](https://github.com/labstack/echo/releases/tag/v4.3.0)
  - [github.com/lib/pq@v1.10.2](https://github.com/lib/pq/releases/tag/v1.10.2)
  - [github.com/gabriel-vasile/mimetype@v1.3.0](https://github.com/gabriel-vasile/mimetype/releases/tag/v1.3.0)
  - `github.com/go-openapi/runtime@v0.19.28`
  - [github.com/rs/zerolog@v1.22.0](https://github.com/rs/zerolog/releases/tag/v1.22.0)
  - `github.com/rubenv/sql-migrate@v0.0.0-20210408115534-a32ed26c37ea`
  - `golang.org/x/crypto@v0.0.0-20210513164829-c07d793c2f9a`
  - `golang.org/x/sys@v0.0.0-20210514084401-e8d321eab015`
  - [google.golang.org/api@v0.46.0](https://github.com/googleapis/google-api-go-client/releases/tag/v0.46.0)
- GitHub Actions:
  -  Pin to `actions/checkout@v2.3.4`.
  -  Remove unnecessary `git checkout HEAD^2` in CodeQL step (Code Scanning recommends analyzing the merge commit for best results).
  -  Limit trivy and codeQL actions to `push` against `master` and `pull_request` against `master` to overcome read-only access workflow errors.

## 2021-04-27
### Added
- Adds `test.WithTestDatabaseFromDump*`, `test.WithTestServerFromDump` methods for writing tests based on a database dump file that needs to be imported first:
  - We dynamically setup IntegreSQL pools for all combinations passed through a `test.DatabaseDumpConfig{}` object:
    - `DumpFile string` is required, absolute path to dump file
    - `ApplyMigrations bool` optional, default `false`, automigrate after installing the dump
    - `ApplyTestFixtures bool` optional, default `false`, import fixtures after (migrating) installing the dump
  - `test.ApplyDump(ctx context.Context, t *testing.T, db *sql.DB, dumpFile string) error` may be used to apply a dump to an existing database connection.
  - As we have dedicated IntegreSQL pools for each combination, testing performance should be on par with the default IntegreSQL database pool. 
- Adds `test.WithTestDatabaseEmpty*` methods for writing tests based on an empty database (also a dedicated IntegreSQL pool). 
- Adds context aware `test.WithTest*Context` methods reusing the provided `context.Context` (first arg).
- Adds `make sql-dump` command to easily create a dump of the local `development` database to `/app/dumps/development_YYYY-MM-DD-hh-mm-ss.sql` (.gitignored).

### Changed
- `test.ApplyMigrations(t *testing.T, db *sql.DB) (countMigrations int, err error)` is now public (e.g. for usage with `test.WithTestDatabaseEmpty*` or `test.WithTestDatabaseFromDump*`)
- `test.ApplyTestFixtures(ctx context.Context, t *testing.T, db *sql.DB) (countFixtures int, err error)` is now public (e.g. for usage with `test.WithTestDatabaseEmpty*` or `test.WithTestDatabaseFromDump*`)
- `internal/test/test_database_test.go` and `/app/internal/test/test_server_test.go` were massively refactored to allow for better extensibility later on (non breaking, all method signatures are backward-compatible).  

## 2021-04-12
### Added
- Adds echo `NoCache` middleware: Use `middleware.NoCache()` and `middleware.NoCacheWithConfig(Skipper)` to explicitly force browsers to never cache calls to these handlers/groups.

### Changed
- `/swagger.yml` and `/-/*` now explicity set no-cache headers by default, forcing browsers to re-execute calls each and every time.
- Upgrade [watchexec@v1.15.0](https://github.com/watchexec/watchexec/releases/tag/1.15.0) (requires `./docker-helper.sh --rebuild`).

## 2021-04-08
### Added
- Live-Reload for our swagger-ui is now available out of the box: 
  - [allaboutapps/browser-sync](https://hub.docker.com/r/allaboutapps/browser-sync) acts as proxy at [localhost:8081](http://localhost:8081/).
  - Requires `./docker-helper.sh --up`.
  - Best used in combination with `make watch-swagger` (still refreshes `make all` or `make swagger` of course).

### Changed
- Upgrades to [swaggerapi/swagger-ui:v3.46.0](https://github.com/swagger-api/swagger-ui/tree/v3.46.0) from [swaggerapi/swagger-ui:v3.28.0](https://github.com/swagger-api/swagger-ui/compare/v3.28.0...v3.46.0)
- Upgrades to [github.com/labstack/echo@v4.2.2](https://github.com/labstack/echo/releases/tag/v4.2.2)
- `golang.org/x/crypto v0.0.0-20210322153248-0c34fe9e7dc2`
- Upgrades to [google.golang.org/api@v0.44.0](https://github.com/googleapis/google-api-go-client/releases/tag/v0.44.0)

## 2021-04-07
### Changed
-  Moved `/api/main.yml` to `/api/config/main.yml` to overcome path resolve issues (`../definitions`) with the VSCode [42crunch.vscode-openapi](https://github.com/42Crunch/vscode-openapi) extension (auto-included in our devContainer) and our go-swagger concat behaviour. 
- Updated [api/README.md](https://github.com/allaboutapps/go-starter/blob/master/api/README.md) information about `/api/swagger.yml` generation logic and changed `make swagger-concat` accordingly

## 2021-04-02
### Changed
- Bump [golang from v1.16.2 to v1.16.3](https://github.com/golang/go/issues?q=milestone%3AGo1.16.3+label%3ACherryPickApproved) (requires `./docker-helper.sh --rebuild`).

## 2021-04-01
### Changed
- Bump golang.org/x/crypto@v0.0.0-20210322153248-0c34fe9e7dc2
- Bump golang.org/x/sys@v0.0.0-20210331175145-43e1dd70ce54
- Bump [github.com/go-openapi/swag@v0.19.15](https://github.com/allaboutapps/go-starter/pull/71)
- Bump [github.com/go-openapi/strfmt@v0.20.1](https://github.com/allaboutapps/go-starter/pull/70)

## 2021-03-30
### Changed
- Bump [github.com/gotestyourself/gotestsum@v1.6.3](https://github.com/gotestyourself/gotestsum/releases/tag/v1.6.3) (requires `./docker-helper.sh --rebuild`).

## 2021-03-26
### Changed
- Bump [golangci-lint@v1.39.0](https://github.com/golangci/golangci-lint/releases/tag/v1.39.0) (requires `./docker-helper.sh --rebuild`).

## 2021-03-25
### Changed
- Bump github.com/rs/zerolog from [1.20.0 to 1.21.0](https://github.com/allaboutapps/go-starter/pull/69)
- Bump google.golang.org/api from [0.42.0 to 0.43.0](https://github.com/allaboutapps/go-starter/pull/68)

## 2021-03-24

### Changed
- We no longer do explicit calls to `t.Parallel()` in our go-starter tests (except autogenerated code). For the reasons why see [FAQ: Should I use `t.Parallel()` in my tests?](https://github.com/allaboutapps/go-starter/wiki/FAQ#should-i-use-tparallel-in-my-tests).
- Switched to [github.com/uw-labs/lichen](https://github.com/uw-labs/lichen) for getting license information of embedded dependencies in our final `./bin/app` binary.
- The following make targets are no longer flagged as `(opt)` and thus move into the main `make help` target (use `make help-all` to see all targets): 
  - `make lint`: Runs golangci-lint and make check-*.
  - `make go-test-print-slowest`: Print slowest running tests (must be done after running tests).
  - `make get-licenses`: Prints licenses of embedded modules in the compiled bin/app.
  - `make get-embedded-modules`: Prints embedded modules in the compiled bin/app.
  - `make clean`: Cleans ./tmp and ./api/tmp folder.
  - `make get-module-name`: Prints current go module-name (pipeable).
- `make check-gen-dirs` now ignores `.DS_Store` within `/internal/models/**/*` and `/internal/types/**/*` and echo an errors detailing what happened.
- Upgrade to [`github.com/go-openapi/runtime@v0.19.27`](https://github.com/go-openapi/runtime/compare/v0.19.26...v0.19.27)
## 2021-03-16
### Changed
- `make all` no longer executes `make info` as part of its targets chain.
  - It's very common to use `make all` multiple times per day during development and thats fine! However, the output of `make info` is typically ignored by our engineers (if they explicitly want this information, they use `make info`). So `make all` was just too spammy in it's previous form.
  - `make info` does network calls and typically takes around 5sec to execute. This slowdown is not acceptable when running `make all`, especially if the information it provides isn't used anyways.
  - Thus: Just trigger `make info` manually if you need the information of the `[spec DB]` structure, current `[handlers]` and `[go.mod]` information. Furthermore you may also visit `tmp/.info-db`, `tmp/.info-handlers` and `tmp/.info-go` after triggering `make info` as we store this information there after a run. 

## 2021-03-15
### Changed
- Upgrades `go.mod`:
  - [`github.com/volatiletech/sqlboiler/v4@v4.5.0`](https://github.com/volatiletech/sqlboiler/blob/master/CHANGELOG.md#v450---2021-03-14)
  - [`github.com/rogpeppe/go-internal@v1.8.0`](https://github.com/rogpeppe/go-internal/releases/tag/v1.8.0)
  - `golang.org/x/crypto@v0.0.0-20210314154223-e6e6c4f2bb5b`
  - ~`golang.org/x/sys@v0.0.0-20210314195730-07df6a141424`~
  - `golang.org/x/sys@v0.0.0-20210315160823-c6e025ad8005`
  - [`google.golang.org/api@v0.42.0`](https://github.com/googleapis/google-api-go-client/releases/tag/v0.42.0)
- `make help` no longer reports `(opt)` flagged targets, use `make help-all` instead.
- `make tools` now executes `go install {}` in parallel
- `make info` now fetches information in parallel
- Seeding: Switch to `db|dbUtil.WithTransaction` instead of manually managing the db transaction. *Note*: We will enforce using `WithTransaction` instead of manually managing the life-cycle of db transactions through a custom linter in an upcoming change. It's way safer and manually managing db transactions only makes sense in very very special cases (where you will be able to opt-out via linter excludes). Also see [What's `WithTransaction`, shouldn't I use `db.BeginTx`, `db.Commit`, and `db.Rollback`?](https://github.com/allaboutapps/go-starter/wiki/FAQ#whats-withtransaction-shouldnt-i-use-dbbegintx-dbcommit-and-dbrollback).

=======
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