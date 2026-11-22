# AGENTS.md: github-following-analyzer

`github-following-analyzer` is a read-only Nushell command that finds non-reciprocal GitHub follows, with configured allowlist exemptions.

## Read before you edit

| When the change touches           | Read                                                                                                                              |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| CI or a workflow                  | [devkit's CI guide](https://github.com/hakulab/devkit/blob/v3/docs/ci.md)                                                       |
| API requests or response handling | [GitHub REST users](https://docs.github.com/en/rest/users/users) and [followers](https://docs.github.com/en/rest/users/followers) |

## Project contracts

- Honor the configured allowlist unless the user asks for an override. Compare exemptions by current login without regard to case, after computing reciprocity by stable numeric ID.
- Let gh own credentials, pagination, and read-only GitHub.com requests. Identify the current account once and pin both collection requests to that login.
- Fetch both complete lists before reporting. A failed page or malformed response aborts the report and never becomes an empty list.
- Keep following and follower counts based on all distinct fetched IDs. Count only excluded non-reciprocal rows as `allow_listed_count`.
- Preserve stdout for the complete report and stderr for summaries and errors. Failures leave stdout empty and exit nonzero. An empty actionable list prints `No accounts to review.`
- Keep credentials in gh or ignored `.env.local`, and keep config, API responses, login lists, and results local. Never persist reports automatically or include personal data in fixtures or public logs.
- Keep tests offline and independent of local config. Use invented data and the adjacent `gh.stub.nu` through devkit's `nu-test` runner.

## Verification

In each new checkout or worktree, run `nix develop -c true` once to install the hooks and link their configuration.

```bash
nix develop -c true
nix flake check --all-systems
nix develop -c run-hooks
nix develop -c nu-test src/github-following-analyzer.test.nu
```

When the package is built, verify its entry point with `nix run . -- --help`.
