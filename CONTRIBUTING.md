# Contributing to a techsilon-oss kit

Thanks for helping. The kits are small, and changes stay small and reviewable. A kit with its own `CONTRIBUTING.md` adds kit-specific steps (how to run its tests, how it releases); read that one too.

1. **Open an issue first** for anything beyond a typo, so the change and its reason are agreed before code.
2. **Branch from the default branch** and open a pull request. Protected branches only move through reviewed PRs.
3. **Conventional commits:** `type(scope): description`, for example `fix(doctor): …`.
4. **Sign off every commit** with `git commit -s`. The `Signed-off-by:` line is your statement under the [Developer Certificate of Origin](https://developercertificate.org/) that you have the right to submit the change under the kit's license (Apache-2.0). CI checks every commit in the pull request.
5. **Tests with the change**, and CI green.
6. **Does it belong in another kit?** Each kit's README says what it owns. Fix a problem in the repo that owns the code, not in a copy.

Every kit follows the [kit standard](KIT-STANDARD.md).

Security problems: see [SECURITY.md](SECURITY.md), not a public issue.
