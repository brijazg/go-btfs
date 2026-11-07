# BTFS 4.1.1 Candidate — Tester Guide

This is a request for testing, not a stable-release announcement. Test only in a disposable environment with backed-up, non-production data.

## Before testing

Record:
- OS and version, CPU architecture, and available memory.
- Go version (`go version`) if building from source.
- Git commit (`git rev-parse HEAD`) and branch.
- Whether this is a fresh install or an upgrade from BTFS 4.1.0.
- Network selection (mainnet/testnet/private network), without publishing credentials.

Do not share seed phrases, private keys, keystore files, API credentials, or unredacted configuration.

## Build and baseline checks

From the repository root:

```sh
go version
go mod verify
go test ./...
go vet ./...
go build ./...
```

Record the complete failing package and relevant error output. A failure caused by a missing system dependency should be distinguished from a source-code failure.

## Functional test checklist

- [ ] Fresh initialization in a disposable repository.
- [ ] Daemon starts and reports its expected version.
- [ ] Daemon stops cleanly and can be restarted.
- [ ] Add a small non-sensitive test file.
- [ ] Retrieve the file and compare its checksum with the original.
- [ ] Pin and unpin test content, then verify the reported state.
- [ ] Verify gateway/API behavior using the documented configuration.
- [ ] Restart the node and verify expected repository state persists.
- [ ] Test network reconnection after a temporary disconnect.
- [ ] Check logs for crashes, repeated errors, or unexpected resource growth.

## Upgrade and compatibility tests

Only use a disposable BTFS 4.1.0 repository copy:
- [ ] Back up the test repository before upgrading.
- [ ] Follow the documented upgrade procedure.
- [ ] Confirm the node starts and existing test content remains accessible.
- [ ] Confirm configuration and identity are preserved.
- [ ] Confirm rollback instructions work on the disposable copy.

Do not experiment on a node containing valuable data or production credentials.

## Report a problem

Use the GitHub issue template **BTFS 4.1.1 tester feedback**. Include reproduction steps, expected/actual behavior, OS/architecture, commit, and sanitized logs. Remove IP addresses, tokens, private paths, personal information, and other secrets before posting.

## What is especially useful to review

- Install and upgrade reliability.
- Data integrity and repository migration.
- Stability during long-running daemon operation.
- API/gateway security and safe defaults.
- CPU, RAM, disk, and network usage under representative workloads.
- Dependency health, reproducible builds, and release artifact integrity.
- Documentation clarity for first-time node operators.
