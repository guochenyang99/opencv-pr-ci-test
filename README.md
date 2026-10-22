# OpenCV PR CI sandbox

This public sandbox verifies the authorization and Git integration flow for
HYGON-AI/opencv-python-das PR #5. Its default branch is `4.9.0`.

## Scope

- The active workflow uses GitHub-hosted Ubuntu runners only.
- Authorization, triggers, concurrency, exact base/head checkout and merge,
  and final-result logic are copied from production without changes.
- The production GPU job is replaced by a checkout/merge smoke job.
- No GPU compilation, submodule checkout, dependency installation, wheel repair,
  or wheel tests are performed. A green check is only a smoke-test result.
- `ci-reference/pr-build.yml` is the complete production candidate, stored
  outside the workflows directory so it cannot run.
- No production variables, secrets, or runners are needed.

## Scenarios

1. Open a same-repository PR into `4.9.0` without `ready-build`: it should pass.
2. Push another commit to that PR without a label: it should pass again.
3. Add or remove an unrelated label: no smoke job should run for that event.
4. Add a `smoke-fail` file in the PR: the smoke job and final result should fail.
5. Remove the `smoke-fail` file: the next update should pass.
6. From a second account, fork this repository and open a PR: it should be denied.
7. Apply `ready-build` as a maintainer: that fork PR should pass.
8. Push to the same fork PR: it should pass without reapplying the label.
9. Remove `ready-build`: that fork PR and its later updates should be denied.

Fork scenarios require a second GitHub account; they are not covered by the
same-repository PR smoke test.
