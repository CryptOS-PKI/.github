# Contributing to CryptOS-PKI

Thanks for your interest in CryptOS-PKI. This guide covers every repository in the
organization. Some repositories add their own setup steps in their own `CONTRIBUTING.md`;
those steps come on top of this guide.

Everyone taking part follows the [Code of Conduct](CODE_OF_CONDUCT.md). Security problems
are never reported in public: see the [security policy](SECURITY.md).

## Start with an issue

Every change starts as an issue that says what should change and why. Open it from one of
the repository's issue templates. For a large change, agree the approach on the issue
before writing the code. Work that takes several steps gets a parent issue with
ordered sub-issues.

## Branches and commits

- Branch from the repository's default branch (`main`) as `<type>/<issue#>-<slug>`, for
  example `fix/47-tpm-unseal-pcr-drift`.
- Write every commit message as a [Conventional Commit](https://www.conventionalcommits.org/):
  `<type>[optional scope]: <description>`. The types are `feat`, `fix`, `docs`, `style`,
  `refactor`, `perf`, `test`, `build`, `ci`, `chore` and `revert`. A breaking change adds
  `!` after the type or scope and a `BREAKING CHANGE:` footer that explains the migration.
- Sign off every commit (see [Developer Certificate of Origin](#developer-certificate-of-origin)).
  The sign-off is the only trailer a commit carries.
- No emoji in source files or commit messages. Emoji are fine in Markdown.

## Developer Certificate of Origin

CryptOS-PKI accepts contributions under the
[Developer Certificate of Origin](https://developercertificate.org/) (DCO), version 1.1,
instead of a contributor licence agreement. By signing off a commit you certify the
statements below for that contribution. In short: you wrote it, or you otherwise have the
right to submit it under the project's open source licence (Apache License 2.0), and you
accept that the contribution and your sign-off are public and kept permanently.

Sign off with `git commit -s`. Git adds a trailer built from your `user.name` and
`user.email`, and it has to match the commit's author:

```text
Signed-off-by: Jane Doe <jane@example.com>
```

Every repository runs a DCO check on pull requests. It fails when any commit, other than a
merge commit, has no `Signed-off-by:` line matching its author's name and email. To sign
off commits you have already pushed, run `git rebase --signoff origin/main` and force-push
your branch.

The full text of the certificate:

```text
Developer Certificate of Origin
Version 1.1

Copyright (C) 2004, 2006 The Linux Foundation and its contributors.

Everyone is permitted to copy and distribute verbatim copies of this
license document, but changing it is not allowed.


Developer's Certificate of Origin 1.1

By making a contribution to this project, I certify that:

(a) The contribution was created in whole or in part by me and I
    have the right to submit it under the open source license
    indicated in the file; or

(b) The contribution is based upon previous work that, to the best
    of my knowledge, is covered under an appropriate open source
    license and I have the right under that license to submit that
    work with modifications, whether created in whole or in part
    by me, under the same open source license (unless I am
    permitted to submit under a different license), as indicated
    in the file; or

(c) The contribution was provided directly to me by some other
    person who certified (a), (b) or (c) and I have not modified
    it.

(d) I understand and agree that this project and the contribution
    are public and that a record of the contribution (including all
    personal information I submit with it, including my sign-off) is
    maintained indefinitely and may be redistributed consistent with
    this project or the open source license(s) involved.
```

## Pull requests

- **One concern per pull request.** Keep unrelated fixes, refactors and features apart,
  even small ones.
- **Open it as a draft.** CI skips draft pull requests, so run the repository's checks
  locally first (the tasks in its `Taskfile.yml`, where it has one), then mark the pull request
  ready for review when it is finished. That starts CI.
- **Title it as a Conventional Commit.** Pull requests are squash-merged, and the title
  becomes the commit on `main`.
- **Fill in the template** and reference the issue (`Closes #12`).
- **CI must be green** before a maintainer merges.

## Documentation ships with the change

Any change that adds, removes or changes something a user sees (behaviour, configuration,
CLI flags, API fields, errors or setup) updates the documentation in the same change:

- The user documentation lives in [CryptOS-PKI/docs](https://github.com/CryptOS-PKI/docs).
  A user-facing change in `cryptos`, `api`, `manager`, `web` or `helm` comes with a
  companion pull request there, the two linked to each other and merged together.
- The repository's `README.md` stays current in the same pull request.
- A change with nothing user-facing says "no docs change" in its pull request description.

## Licence

CryptOS-PKI is licensed under the [Apache License 2.0](LICENSE). By contributing you agree
that your contributions are licensed under it too.
