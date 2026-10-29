# techsilon-oss kit standard

Every kit in [techsilon-oss](https://github.com/techsilon-oss) reads and acts the same way, so someone who learned one kit finds the same things in the same places in the next.

This file is the standard. Each kit's README links here, and each kit has a checklist issue tracking what it still lacks. Change the standard here, in a PR. Never in a kit's own copy, because kits don't keep one.

**Two kinds of kit.** Rules under [All kits](#all-kits) apply to every repo. Rules under [Plugin kits](#plugin-kits) apply only to kits that ship a Claude Code plugin (govkit, qa-kit). release-gate-kit and gh-runner-kit are not plugins.

---

## All kits

### Files
| File | What it holds |
|---|---|
| `README.md` | Quick start (three lines at most), **what it does on your machine** (reads, writes, network, what it never does), **how to uninstall**, a module or feature table, links to the docs, a link to this standard, and a footer naming the publisher and maintainer |
| `LICENSE` | MIT, "Copyright (c) <year> Techsilon LLC" |
| `CHANGELOG.md` | [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Every release; a release that changes files in a user's repo names them |
| `docs/UPGRADING.md` | From the first breaking change on: one section per breaking release, with exact steps |
| `docs/FAQ.md` | Short answers that link to the guides. The guides own the detail |
| `.github/CODEOWNERS` | The maintainer |
| `CONTRIBUTING.md`, `SECURITY.md`, issue templates | **From this repo's defaults** (GitHub applies them to every repo in the org that has none of its own). A kit keeps its own only when it says something kit-specific; GitHub applies issue templates all-or-nothing, so a kit with its own template folder writes all of them |

### Owner fields
In every manifest the kit has (`plugin.json`, `marketplace.json`, `package.json`): author or owner **Techsilon LLC**, email **oss@techsilon.com**, plus `homepage` and `repository`.

### Branch protection
`main` (and a long-lived working branch, if the kit has one) is protected:
- **a pull request is required**, with 0 required approvals while the kit has a single maintainer (nobody can approve their own PR);
- **no force pushes, no deletion**;
- **the CI check is required** where CI exists (`strict: false`);
- admins included (`enforce_admins: true`).

### Private vulnerability reporting
**Enabled** on every kit (Settings → Security → Private vulnerability reporting). The org [SECURITY.md](SECURITY.md) sends reporters to the repo's **Report a vulnerability** button first, and to oss@techsilon.com only as a fallback.

### CI
On every pull request: a manifest check (`claude plugin validate` for a plugin kit, the package manifest otherwise), the tests, and the kit's own guardrails. The check a branch rule requires keeps its name across renames.

### Releases
[Semver](https://semver.org/). Below 1.0, a breaking change is a minor bump and gets an UPGRADING section. **Every release has a `v<version>` git tag and a GitHub Release**, and the tag equals the version in the manifest. A rename keeps the old name working for a stated period, given as a date, not a version.

### License changes
A kit that changes license says so in its README and CHANGELOG, in these words: "MIT from version <next>. Earlier releases were published under <old license> and stay <old license>."

### Upstream never names a consumer
A kit's docs, templates and code never name a repo that uses the kit. Conditions about specific consumers live in the kit's issues.

---

## Plugin kits

### Names
- **Every skill name starts with the kit's prefix** (`govkit-*`, `qa-*`). A kit never ships a bare name like `/sdlc`. A project's own skill of that name would clash with it, and the wrong one would answer.
- **The `/plugin` description** is one line in user language, ending "Help: /<kit>-help".
- **A `/<kit>-help` skill** answers from the docs that ship in the plugin, for the installed version.

### Module notes
Each module, or the kit as a whole, has a note: what you get, what it changes outside the repo, what it costs, how to undo it, and a "Read more" link. It's in plain user language and **shown before the user says yes**. It is kept **once**, in `module.json`, and the README table, init and doctor read it or are generated from it, so they can't drift apart. Each module has a guide in `docs/modules/<module>.md`.

### Links at every touch point
The module note links to its guide; init ends with next steps and links; **every doctor finding links to its fix**.

### Security rules
Learned in govkit 0.9.0–0.9.1. A skill's `allowed-tools` runs commands without asking, so:
- **Pre-approve only the plugin's own scripts**, by their path under `${CLAUDE_PLUGIN_ROOT}`, quoted as the skill invokes them. **Never** a wildcard before a script name (`node *x.mjs`). It matches a repo's look-alike file.
- **Never pre-approve `npm`/`pnpm`/`yarn`/`npx` scripts:** `package.json` is the repo's.
- **`git config --get` only**, never `git config *` (writes such as `core.sshCommand` run code later).
- **Repo-authored instructions or commands** (a project's own steps, hooks, config) are used **only after the user trusts that exact file**, by hash, recorded outside version control. **The user does the trusting, never the agent**, and the file is shown to the agent as data between markers, never as instructions.
- **A test enforces the pre-approval rules** for every skill.

---

_Adopted 2026-10-29 (govkit#16). Kit checklists: one issue per kit, linked from govkit#16._
