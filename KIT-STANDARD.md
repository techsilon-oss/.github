# techsilon-oss kit standard

Every kit in [techsilon-oss](https://github.com/techsilon-oss) reads and acts the same way, so someone who learned one kit finds the same things in the same places in the next.

This file is the standard. Each kit's README links here, and each kit has a checklist issue tracking what it still lacks. Change the standard here, in a PR. Never in a kit's own copy, because kits don't keep one.

**Two kinds of kit.** Rules under [All kits](#all-kits) apply to every repo. Rules under [Plugin kits](#plugin-kits) apply only to kits that ship a Claude Code plugin (govkit, qa-kit, docs-kit). release-gate-kit and gh-runner-kit are not plugins.

**The command line is the product.** Every kit works with Git and its runtime alone, in any terminal or CI. A Claude Code plugin is an optional layer over the same commands, never the only way in.

---

## All kits

### Files
| File | What it holds |
|---|---|
| `README.md` | In this order: **what it is** (one sentence), **what you get** (three to five results, see [Documentation](#documentation)), **quick start** (three lines at most), **what it does on your machine** (what it reads and writes, every network call, what it never does, what it depends on, and how to pin a release by tag or commit, or vendor it), **how to uninstall**, a module or feature table, links to the docs, a link to this standard, and a footer naming the publisher and maintainer and linking [TRADEMARKS.md](TRADEMARKS.md) |
| `LICENSE` | Apache License 2.0, unchanged text |
| `NOTICE` | Three lines: the kit's name; "Copyright <year> Techsilon LLC"; "This product includes software developed by Techsilon LLC (https://github.com/techsilon-oss/<kit>)." Under Apache-2.0 §4(d), whoever redistributes the kit keeps this file |
| `CHANGELOG.md` | [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Every release; a release that changes files in a user's repo names them |
| `docs/UPGRADING.md` | From the first breaking change on: one section per breaking release, with exact steps |
| `docs/FAQ.md` | Short answers that link to the guides. The guides own the detail. It answers "What if the maintainer stops?": the license lets anyone fork, and the answer says so plainly |
| `.github/CODEOWNERS` | The maintainer |
| `CONTRIBUTING.md`, `SECURITY.md`, issue templates | **From this repo's defaults** (GitHub applies them to every repo in the org that has none of its own). A kit keeps its own only when it says something kit-specific; GitHub applies issue templates all-or-nothing, so a kit with its own template folder writes all of them |

### Owner fields
In every manifest the kit has (`plugin.json`, `marketplace.json`, `package.json`): author or owner **Techsilon LLC**, email **oss@techsilon.com**, plus `homepage` and `repository`.

### Branch and tag protection
`main` (and a long-lived working branch, if the kit has one) is protected:
- **a pull request is required**, with 0 required approvals while the kit has a single maintainer (nobody can approve their own PR);
- **no force pushes, no deletion**;
- **the CI check is required** where CI exists (`strict: false`);
- admins included (`enforce_admins: true`).

**Release tags never move.** A tag ruleset on `refs/tags/v*` blocks update and deletion, so a pinned `v<version>` always means the same code. A bad release is fixed by a new release, never by moving its tag.

### Private vulnerability reporting
**Enabled** on every kit (Settings → Security → Private vulnerability reporting). The org [SECURITY.md](SECURITY.md) sends reporters to the repo's **Report a vulnerability** button first, and to oss@techsilon.com only as a fallback, and states the response times.

### Contributions: DCO
Every commit in a pull request carries a `Signed-off-by:` line (`git commit -s`): the [Developer Certificate of Origin](https://developercertificate.org/), the author's statement that they have the right to submit the change under the kit's license. CI checks it with this repo's reusable workflow ([`.github/workflows/dco.yml`](.github/workflows/dco.yml)). No contributor license agreement is asked for.

### CI
On every pull request: a manifest check (`claude plugin validate` for a plugin kit, the package manifest otherwise), the tests, the kit's own guardrails, the DCO check, and the plain-words check ([Documentation](#documentation)). The check a branch rule requires keeps its name across renames.

### Releases
[Semver](https://semver.org/). Below 1.0, a breaking change is a minor bump and gets an UPGRADING section. **Every release has a `v<version>` git tag and a GitHub Release**, and the tag equals the version in the manifest. A rename keeps the old name working for a stated period, given as a date, not a version.

### License
**Apache License 2.0** for every kit, with a `NOTICE` file. It is as open as MIT, and adds two things a company's legal review looks for: an explicit patent grant, and a `NOTICE` that travels with every redistributed copy, so the credit to Techsilon LLC stays with the code. Each file's own copyright line is not required; the `LICENSE` and `NOTICE` at the root cover the kit.

A kit that changes license says so in its README and CHANGELOG, in these words: "<new license> from version <next>. Earlier releases were published under <old license> and stay <old license>." Kits published under MIT move to Apache-2.0 in their next release.

### Names and trademarks
The license covers the code, not the names. "Techsilon" and the kit names are trademarks of Techsilon LLC, as [TRADEMARKS.md](TRADEMARKS.md) says. A fork may use the code under the license; it uses its own name.

### Upstream never names a consumer
A kit's docs, templates and code never name a repo that uses the kit. Conditions about specific consumers live in the kit's issues.

### Documentation
A reader who has never seen the kit reads the README and knows, in a minute, what they get and how to start. Each rule below can be checked.

**What you get comes first.** The README says what the kit does for the reader as results they can see: "A test fails when a doc is in the wrong place", not "Powerful layout validation". Three to five results. Features, options and exit codes go in the reference, after the quick start.

**Where the docs live.** `docs/` follows the [docs-kit](https://github.com/techsilon-oss/docs-kit) layout: a front door (`docs/README.md`), one folder per subject with a `README.md` map, how-tos in `<subject>/howto/HOWTO-<task>.md`, and the reference. `docs-kit check` runs in CI. Every kit has at least these how-tos: adopt it, upgrade it, remove it.

**How it is written: ASD-STE100 (Simplified Technical English).** For every how-to and every quick start, each rule is mandatory; for the rest, write in its spirit:
- One action per step. Number the steps.
- Imperative mood: "Run", "Open", "Add".
- The condition before the instruction: "If the check fails, run …".
- One word for one meaning. Use the exact name of the command, file or button, in code format or **bold**. No synonyms for variety.
- Short sentences, active voice, the simple and common word.
- A warning before the step it applies to.
- Say where each step happens: which program, which window, which folder.
- Never a secret in a doc. Say where the reader keeps it.

**No filler.** No sales words and no padding. A sentence that would be true of any product ("makes your workflow easier") is cut. The plain-words check fails a README or a doc under `docs/` that uses a filler word, such as "seamless", "robust" or "leverage", outside code. The list is kept once, in this repo's [`.github/workflows/plain-words.yml`](.github/workflows/plain-words.yml), which each kit's CI calls. To add a word, change it there.

**Examples run.** Every command shown in the README and in a how-to is one a test runs, or is marked as an illustration (`# example:`). A doc never shows output the kit no longer prints.

**No drifting numbers.** A doc does not state counts or versions that change (tests, files, findings); it names the command that reports them. The version appears only where the reader pins it.


---

## Plugin kits

### Names
- **Every skill name starts with the kit's prefix** (`govkit-*`, `qa-*`, `docs-kit-*`). A kit never ships a bare name like `/sdlc`. A project's own skill of that name would clash with it, and the wrong one would answer.
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

_Adopted 2026-10-29 (govkit#16). Amended 2026-11-13: the command line is the product; Apache-2.0 and `NOTICE`; names and trademarks; DCO; release tags never move; Documentation. Kit checklists: one issue per kit, linked from govkit#16._
