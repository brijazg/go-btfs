# BTFS 4.1.1 — Maintenance Review (Draft)

**Status:** Request for testing and technical review. This is not a stable release.
**Base:** BTFS 4.1.0 source, kept compatible with the upstream project.
**Branch:** `maintenance/btfs-4.1.1-rc1`

## Testers and reviewers wanted

We need independent testers, Go developers, node operators, and security reviewers to help evaluate this maintenance candidate.

Please review the changes, run the checks in [TESTING-4.1.1-RC1.md](TESTING-4.1.1-RC1.md), and report:
- Operating system and version, CPU architecture, and Go version.
- Exact command and steps to reproduce a problem.
- Expected result and actual result.
- Relevant sanitized logs and whether the issue is repeatable.
- Whether the result is a regression from BTFS 4.1.0.

**Never post seed phrases, private keys, keystore files, tokens, credentials, or personal data.** Use a disposable test environment and non-sensitive test files. Do not test with funds or production data.

## Current scope

This review branch adds maintenance documentation, a repeatable CI baseline, and a structured tester feedback form. It does not yet claim to fix every BTFS runtime issue, upgrade every dependency, or certify a production release. No new binary or stable release is published by this branch.

The application version remains 4.1.0 until code-level changes are reviewed, tested, and approved. The label 4.1.1 refers to the proposed maintenance cycle, not a claim that a 4.1.1 binary exists.

## Initial review: what should be improved

### Priority 0 — release safety
- Establish passing Linux build and test results before publishing binaries.
- Add repeatable upgrade, fresh-install, daemon-start, shutdown, and data-persistence tests.
- Verify BTFS 4.1.0 repository compatibility and confirm existing node data is preserved.
- Review key handling, API/gateway exposure, authentication, permissions, and network-facing defaults.
- Document rollback and backup/restore procedures.

### Priority 1 — supply chain and security
- Run `govulncheck` and review findings rather than blindly upgrading dependencies.
- Review direct and transitive dependencies for maintenance status and known vulnerabilities.
- Generate an SBOM for release artifacts and document provenance/checksums.
- Pin GitHub Actions to reviewed versions or immutable commit SHAs.
- Set least-privilege workflow permissions and add workflow timeouts/concurrency controls.
- Add secret scanning and dependency update automation where maintainers can review changes.

### Priority 2 — CI and code quality
- Run `go mod verify`, `go test ./...`, `go vet ./...`, and `go build ./...` in CI.
- Add platform-specific build coverage where supported by the project.
- Add race tests for concurrency-sensitive packages where practical.
- Track flaky, skipped, and environment-dependent tests separately.
- Keep CI failures visible; do not mark a candidate stable while required checks fail.

### Priority 3 — usability and operations
- Improve installation and upgrade documentation for Linux, macOS, Windows binaries, and Docker.
- Document minimum/recommended hardware, disk growth, bandwidth, ports, and backup needs.
- Provide clear diagnostics that redact secrets by default.
- Explain storage-provider setup, network selection, troubleshooting, and recovery.
- Publish a compatibility matrix for node versions, network, APIs, and operating systems.

## Known limitations of this review branch

- The upstream source tree is large; a full local build and runtime test has not been completed here.
- CI results must be reviewed after GitHub Actions runs.
- Dependency findings require triage; automated scanners can report false positives and do not replace review.
- No production deployment or live network behavior is certified by this document.

## Acceptance criteria for a stable maintenance release

1. Required CI jobs pass on supported environments.
2. No unresolved critical/high security finding without a documented, approved mitigation.
3. Fresh install and upgrade tests pass, including preservation of existing repository data.
4. Core upload/download, retrieval, pinning, daemon restart, and gateway tests pass.
5. Reviewers confirm protocol and network compatibility with the intended BTFS network.
6. Release artifacts are reproducible or have documented provenance and published checksums.
7. Release notes clearly state changes, known issues, upgrade steps, and rollback guidance.
