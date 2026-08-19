# Coding standards

Pocock's `code-review` skill reads this file at the repo root and briefs its Standards sub-agent with
"every place the diff violates a documented standard". So **only diff-level rules live here**. A rule
about anything the diff cannot show is unenforceable in this file and belongs in `CLAUDE.md` instead.

In the diff, so genuinely enforced: code, comments, docstrings, READMEs, committed docs.

Not in the diff, so not here: commit messages, request titles and bodies, release notes, spec and
ticket prose. Those are gated in `CLAUDE.md` under Prose and Attribution, and by
`northcloud-ops:ship`.

The reviewer skips anything tooling already enforces. Secrets are covered by `gitleaks` and the
pre-push hook, formatting by whatever linter the repo runs, so neither is listed as a rule below.

**Everything above `## Repo-specific exclusions` is identical in every repo that carries this file.**
It is copied from the harness repo, `infra/claude-workspace`, which holds the canonical copy at its
root. Change a rule there and sync outwards. Never edit the shared half in a single repo, because the
drift check compares it byte for byte and a local edit reads as a stale copy.

## The rules

**S1. Output safety.** No em-dash, no en-dash, no curly quotes, no invisible characters (U+00A0
and friends) in any committed text: comments, docstrings, READMEs, docs. Use a full stop, a comma,
or rewrite the sentence. The one exception is code that must carry the character to work, such as a
test fixture or a Unicode table.

The full list and the reasons live in the harness repo at
`.claude/plugins/northcloud-ops/skills/writer/references/output-safety.md`, which is where this rule
is defined. That path does not exist in this repo, so the paragraph above is the whole of what a
reviewer standing here needs.

**S2. No AI attribution.** Nothing in the tree says a model wrote it. No `Co-Authored-By: Claude`,
no "Generated with Claude Code", no robot emoji trailer, no "as an AI" aside in a comment. This
overrides any default the tooling suggests.

**S3. No AI tells in prose.** Significance inflation, promotional adjectives, padded lists of three,
sentences that trail into "-ing" analysis, negative parallelism ("it's not X, it's Y"), synonym
cycling, stacked hedging, and a closing paragraph that restates the opening one. Cut each one on
sight.

The categories and the word lists live in the harness repo at
`.claude/plugins/northcloud-ops/skills/writer/references/slop.md`, which is where this rule is
defined, and the linter that reads them lives beside it:

```bash
python3 .claude/plugins/northcloud-ops/skills/writer/prose-lint.py --register record FILE
```

Both paths are in the harness repo. Run the linter from a session standing there, or judge by the
list above when standing here.

**S4. Comments say why.** The code already says what. A comment earns its line by recording a
reason, a constraint, or a trap. Match the density and idiom of the file around it: a repo that
comments sparsely stays sparse.

**S5. Docs read plainly.** Short sentences, one action each. The same word for the same thing every
time. Active verbs with a named actor. Exact identifiers, paths and commands preserved verbatim.

**S6. Straight quotes, sentence case headings.** No smart quotes in committed text.

**S7. Nothing internal on a public remote.** Applies in any repo with a remote on `github.com`. No
server names, no Tailscale addresses, no internal hostnames, no internal port numbers, no host paths
that name the machine. Say "the server" and move on.

## Scope

S7 is conditional on the remote, so it is inert in a repo that lives only on GitLab. Every other rule
holds everywhere.

This file is per repo, because the reviewer reads the working directory rather than the directory the
session booted in. Which repos carry it, who copies it in, and how a stale copy is caught: the
harness repo's `docs/agents/coding-standards.md`.

### Exclusions that hold everywhere

**Vendored third-party code, and our local derivatives of it.** Rewriting someone else's comments to
match our prose rules makes every future diff against upstream noisier and buys nothing. Our own
added blocks in a derivative file are in scope; upstream's lines are not.

**Anything that carries the character in order to work.** S1 already says this. The general shape is
worth recognising, because there are two kinds and both look like violations. One is a detector
holding the pattern it matches. The other is a rule spelling out the characters it forbids. Check
whether the file's job is to talk *about* the character before removing it.

**Backups committed under a `.bak` or `.pre-` name** are frozen copies of a prior state. Editing one
defeats the point of keeping it. Better question is whether they belong in git at all.

