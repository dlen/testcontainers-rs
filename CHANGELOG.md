# Changelog

All notable changes to this project will be documented in this file.

## [0.16.7] - 2024-05-01
### Details
#### Bug Fixes
- `get_host` for `unix` and `npipe` docker hosts ([#621](https://github.com/testcontainers/testcontainers-rs/pull/621))

#### Features
- Extend `WaitFor` for `ExecCommand` ([#622](https://github.com/testcontainers/testcontainers-rs/pull/622))
## [0.16.6] - 2024-04-30
### Details
#### Features
- Add `get_host` to `Container` and deprecate `get_host_ip_address` ([#618](https://github.com/testcontainers/testcontainers-rs/pull/618))
- Allow cgroupns-mode and userns-mode to be configured  ([#605](https://github.com/testcontainers/testcontainers-rs/pull/605))
## [0.16.5] - 2024-04-29
### Details
#### Bug Fixes
- Correct drop of a network for sync container ([#612](https://github.com/testcontainers/testcontainers-rs/pull/612))
- Correct default for `get_host_ip_address` in case of `unix` or `npipe` ([#613](https://github.com/testcontainers/testcontainers-rs/pull/613))

#### Miscellaneous Tasks
- Update `serde-java-properties` to `0.2.0` ([#614](https://github.com/testcontainers/testcontainers-rs/pull/614))
## [0.16.4] - 2024-04-29
### Details
#### Bug Fixes
- Properly expose mapped ports ([#610](https://github.com/testcontainers/testcontainers-rs/pull/610))

#### Features
- Introduce `properties-config` cargo feature ([#608](https://github.com/testcontainers/testcontainers-rs/pull/608))
- Support docker auth configuration for image pulling ([#609](https://github.com/testcontainers/testcontainers-rs/pull/609))
## [0.16.3] - 2024-04-27
### Details
#### Bug Fixes
- Expose mount-related types ([#603](https://github.com/testcontainers/testcontainers-rs/pull/603))
## [0.16.2] - 2024-04-27
### Details
#### Documentation
- Fix `docsrs` attributes
## [0.16.1] - 2024-04-27
### Details
#### Documentation
- Add readme to crates.io and fix documentation builds ([#598](https://github.com/testcontainers/testcontainers-rs/pull/598))
- Fix symlink to readme ([#600](https://github.com/testcontainers/testcontainers-rs/pull/600))
## [0.16.0] - 2024-04-27
### Details
#### Bug Fixes
- Use the binds option instead of volumes for mounts ([#581](https://github.com/testcontainers/testcontainers-rs/pull/581))

#### Documentation
- Mention a way to preserve running containers ([#586](https://github.com/testcontainers/testcontainers-rs/pull/586))

#### Features
- Support for docker cli flag `--add-host` ([#547](https://github.com/testcontainers/testcontainers-rs/pull/547))
- Allow to override args in a `RunnableImage` ([#558](https://github.com/testcontainers/testcontainers-rs/pull/558))
- Add `name` parameter to `RunnableImage` ([#549](https://github.com/testcontainers/testcontainers-rs/pull/549))
- [❗] Container-centric API with refactored underlying layer ([#575](https://github.com/testcontainers/testcontainers-rs/pull/575))
- Add ability to pull-image explicitly ([#579](https://github.com/testcontainers/testcontainers-rs/pull/579))

#### Miscellaneous Tasks
- Remove `spectral` from `dev-dependencies` ([#526](https://github.com/testcontainers/testcontainers-rs/pull/526))
- Update reqwest requirement from 0.11.14 to 0.12.3 ([#569](https://github.com/testcontainers/testcontainers-rs/pull/569))
- Update bollard and bollard-stubs ([#574](https://github.com/testcontainers/testcontainers-rs/pull/574))
- Reuse workspace level configs ([#568](https://github.com/testcontainers/testcontainers-rs/pull/568))

#### Refactor
- [❗] Api for mounts and volumes ([#596](https://github.com/testcontainers/testcontainers-rs/pull/596))

#### Styling
- Derive `Default` to fix clippy lint ([#525](https://github.com/testcontainers/testcontainers-rs/pull/525))
- Actualize formatting configs ([#567](https://github.com/testcontainers/testcontainers-rs/pull/567))
- Enable `StdExternalCrate` grouping
## [0.15.0] - 2023-09-28

### Added

- Added `privileged` parameter to `RunnableImage`
- Added `shm_size` (shared memory size) parameter to `RunnableImage`
- Added logging of an error when starting a container fails

### Changed

- `Container::exec` changed to be synchronous and return `ExecOutput`
- MSRV is now 1.63.
- Fixed `get_bridge_ip_address` not to use outdated properties
- `Docker` trait is declared to be `Send + Sync`

### Removed

- Removed all pre-defined images from the library to escape unbounded maintenance work.
  See https://github.com/testcontainers/testcontainers-rs/issues/471 for details.
- Removed explicit support for podman.
  See https://github.com/testcontainers/testcontainers-rs/issues/422 for details.

## [0.14.0] - 2022-05-30

### Added

- Added `watchdog` feature that spawns a background thread keeping track of docker containers that are started by the test suite and removes them in the case of a `CTRL+C` or `kill` of the test process.
- Introduced `Container::get_host_port_ipv4`, `Container::get_host_port_ipv6`, `ContainerState::host_port_ipv4`, and `ContainerState::host_port_ipv6` to better handle automatically assigned ports.
  Docker may bind the same exposed container port to different host ports on `0.0.0.0` and `::`, depending on influences from the environment.

### Changed

- `Container::get_host_port` and `ContainerState::host_port` are now deprecated in favor of the new IPv4- and IPv6-specific methods.
- MSRV is now 1.60.

## [0.13.0] - 2022-04-04

### Added

- A new client implementation that talks to the Docker daemon via **HTTP**.
  This implementation is available as `testcontainers::clients::Http` and provides an **async** interface.
  As of now, this implementation is guarded behind the `experimental` feature-flag and not yet guaranteed to work flawlessly.
- Allow using `podman` CLI in addition to `docker`
- The `TESTCONTAINERS` environment variable to control what happens to containers and networks at the end of a test.
  The default value is `remove` which deletes all containers and networks that were used in the test.
  By setting the value to `keep`, containers and networks will not be deleted but kept **running**.
  You will have to **stop** and **delete** those yourself eventually.
- Upgrade default bitcoin-core image version to 0.21.0. This allows us to remove `-debug` for bitcoind and replace it with
  `-startupnotify=echo ...`. More details on bitcoind 0.21.0 can be found [here](https://github.com/bitcoin/bitcoin/blob/master/doc/release-notes/release-notes-0.21.0.md).
  Note: This release also removed the default wallet.
- `expose_port` functionality to `Image` trait.
- `Google Cloud SDK` image
- `RabbitMQ` image
- `WaitFor::Healthcheck` container ready condition, which corresponds with the [healthcheck](https://docs.docker.com/engine/reference/builder/#healthcheck) status.
- `MinIO` image

### Changed

- How images express when a container is ready: Instead of implementing `wait_until_ready`, images now need to implement `ready_conditions` which returns a list of `WaitFor` instances.
- Return value of `get_host_port` from `Option<u16>` to `u16`.
  If the port cannot be resolved, this function will now **panic**.
- MSRV bumped to 1.46.
- Make `Docker` trait `pub(crate)`.
  This reduces the API surface of the crate which allows for fewer breaking changes in the future.
  All functionality from `Docker` (start, stop, rm, and ports) is available on a container directly.
- `descriptor` is broken down into `name` and `tag` within `Image` trait.
- Bump `MongoDB`-image default version to `5.0.6`.

### Removed

- `DYNAMODB_ADDITIONAL_SLEEP_PERIOD` variable from `dynamodb_local` image.
  Previously, we had a fallback of 2 seconds if this variable was not defined.
  We now wait for 2 seconds unconditionally after the specified message has been found.
- Support for the `KEEP_CONTAINERS` env variable.
  The functionality of `KEEP_CONTAINERS=true` is superseded by `TESTCONTAINERS=keep`.
- `with_entrypoint` from the `Image` trait.
  This functionality is not used within the library.
  Images that need this kind of customization can always implement it on their own type directly but there is no need to force it onto them.
- `Image::EnvVars` and `Image::Volumes` associated types.
  The respective functions `Image::env_vars` and `Image::volumes` still exist but now return a trait object that must implement `Iterator<Item = (&String, &String)`.
  This allows us to provide a default implementation which reduces the boilerplate in defining new images.
- `args` and `with_args` from `Image` trait.

### Fixed

- Removing a docker container did not error if failed. This was fixed by asserting the daemon's response instead of
  just the status code: If a docker container was removed correctly using `rm -f -v <ID>` <ID> is printed on stdout.
  <ID> can either be the container name or its ID which is used within testcontainer-rs.
- Fixed clippy warnings of camel case names containing a capitalized acronym.

## [0.12.0] - 2021-01-27

### Added

- Allow custom version for postgres image
- Remove `derivative` dependency
- `OrientDB` image
- `Zookeeper` image

### Changed

- Move port mapping logic to `RunArgs` instead of each Image.

## [0.11.0] - 2020-09-30

### Added

- `Docker::run_with_args` method. This allows naming a container and assigning it to a specific docker network. The docker
  network will be created if it doesn't exist yet. Once the client is dropped, the network will be removed again if it
  has previously been created. A network that already existed will not be removed.
- Address-type argument to `coblox/bitcoin-core` Image.
  We are setting `bech32` as a default here.
  This is different from the default of `bitcoind`.

### Fixed

- Block the thread until containers have been successfully removed.
  Previously, this was done in a fire-and-forget way and hence led to containers not being removed in certain situations.

### Changed

- MSRV is now 1.41.0

## [0.10.0] - 2020-08-20

### Added

- Mongo image.
- Support for the `fallbackfee` argument for the `bitcoin-core` image.
- Ability to customize the `entrypoint` used by the image.
- Ability to start a container once stopped.

### Changed

- MSRV bumped to 1.36 from 1.32.
- Change postgres image authentication POSTGRES_HOST_AUTH_METHOD rather than username and password.
- Bumped bitcoin-core default tag to 0.20.0.

## [0.9.1] - 2020-03-24

### Added

- A changelog
- Support volumes on containers

### Changed

- **Breaking**: `Container#get_host_port` now only accepts a `u16` instead of a `u32`.
  `u16` captures all possible port values.

### Fixes

- Provide a default password for the postgres image.
  There seems to be an unfortunate breaking change in the postgres image that we need to cater for.
