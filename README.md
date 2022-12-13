# test-gitleaks

This repo provides some example configurations for using Gitleaks.

Additionally, its configured in a way that might be true to life - i.e. there are some existing secrets in the repo 
that need to stay for now. However, you want to prevent any new secrets from being introduced

# Deployment options

## On a developer's machine
This repo is already set up to use gitleaks as a pre-commit check as part of the repo.
This is optional - developers will have to opt-in to use this. By default, no checks will run.

To opt-in to use GitLeaks as a pre-commit check do (instructions assume MacOS with homebrew installed):
1. Install pre-commit framework + golang:
  - `brew install pre-commit go`
  - Note: I intend to swap to using the docker runner for precommit in the future, but encountered issues. See https://github.com/zricethezav/gitleaks/issues/1022
2. Configure pre-commit to run on each commit:
  - `pre-commit install`

To see how this will look for developers, create a new file with a secret in the repository and attempt to commit the file. For example:
1. `cp example-secrets-that-already-exist.json new-secrets.json`
2. `git add new-secrets.json`
3. `git commit -m "this should fail!"`

## As part of a CI check
The repo comes configured with some secres that we want to allow for now (see .gitleaks-baseline.json)
However, we don't want to let any others in.

To simulate someone forcing a secret in do something like:
1. Actually force commit a new secret to the repo. Make sure to have completed the steps in the Developer's machine section above first.
  - `SKIP=gitleaks git commit -m "this shouldn't fail"` 
2. Use the `gitleaks detect` command - as would happen in CI
  - `gitleaks detect --baseline-path ./.gitleaks-baseline.json --verbose`
3. Should see that the new secret is found even though it was forced through.


This same type of command could also be used on a periodic schedule rather than in CI to validate repos

