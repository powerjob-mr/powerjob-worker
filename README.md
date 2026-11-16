# powerjob-worker

Worker SDK embedded in applications, its Spring Boot starter, and the official processors.

Split from [PowerJob](https://github.com/PowerJob/PowerJob) at upstream commit
`d92ad66277ca54e14e2835b2e5a6aaf01f620684` (release 5.1.7). The Git history of retained files is preserved,
including earlier names of the modules; filtering rewrites commit IDs.

## Build

Install JDK 8 and Maven 3.9, then run from this repository:

```bash
mvn -B clean verify
```

This repository uses the PowerJob libraries from `powerjob-common`.
Check them out next to this repository:

```text
workspace/
  powerjob-common/
  powerjob-worker/
```

The root `pom.xml` adds each sibling checkout to the Maven reactor, so their
sources are compiled in the same build and changes there are visible here
without installing anything. The parent POM `tech.powerjob:powerjob` comes from
`../powerjob-common`. A sibling that is not checked out is replaced by its
published `5.1.7` artifact from Maven Central.

JDK 8 is the version upstream targets and the version verified locally.
See [build-verification.json](build-verification.json) for the local build results.

## Included modules

- `powerjob-worker/`
- `powerjob-worker-spring-boot-starter/`
- `powerjob-official-processors/`

## Excluded tests

These upstream tests cannot pass in a clean build and are excluded in their
module's surefire configuration. Upstream CI does not run tests.

- `tech.powerjob.worker.test.function.IdleTest` (powerjob-worker): Functional test: needs a running server; one method waits until timed out and the other throws NullPointerException without an initialised worker runtime.
- `tech.powerjob.worker.test.function.MonitorTest#testGetSystemLoadAverage` (powerjob-worker): Prints the load average 10,000 times with a one-second sleep (about 2.8 hours).
- `tech.powerjob.worker.autoconfigure.PowerJobAutoConfigurationTest` (powerjob-worker-spring-boot-starter): Needs a server address: worker initialisation fails with 'serverAddress can't be empty'.
- `tech.powerjob.official.processors.impl.FileCleanupProcessorTest#testCleanWorkerScript` (powerjob-official-processors): Walks the whole filesystem from '/'; in a container it loops on /dev/fd (FileSystemLoopException).
- `tech.powerjob.official.processors.impl.sql.DynamicDatasourceSqlProcessorTest#testProcess` (powerjob-official-processors): Needs MySQL at localhost:3306.
- `tech.powerjob.official.processors.impl.sql.SpringDatasourceSqlProcessorTest` (powerjob-official-processors): Stale: its MySQL-style DDL (bigint(20)) is rejected by H2 2.2.224, the version the build uses.

## Origin

See [UPSTREAM-README.md](UPSTREAM-README.md) for the original project overview
and [split.json](split.json) for the split manifest.

## License

Apache License 2.0; see [LICENSE](LICENSE).
