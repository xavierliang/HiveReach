# Upstream Sync — Panniantong/Agent-Reach → HiveReach

HiveReach is a maintained fork of [Panniantong/Agent-Reach](https://github.com/Panniantong/Agent-Reach), forked at **v1.4.0** on 2026-05-05. Upstream still ships fixes; this file is the playbook for pulling them in without breaking the fork's identity.

## One-time setup

```bash
git remote add upstream https://github.com/Panniantong/Agent-Reach.git
git fetch upstream --tags
```

Tag the fork point so future diffs stay anchored:

```bash
git tag -a fork-base v1.4.0 -m "HiveReach fork point"
```

## Periodic sync workflow

```bash
git fetch upstream
git log fork-base..upstream/main -- hivereach/channels/
```

That command lists every upstream commit that touched a channel file since the fork. Walk it newest → oldest and decide per commit.

## Cherry-pick policy

### ✅ Generally OK to cherry-pick

| Path | Why |
|------|-----|
| `hivereach/channels/*.py` (bugfixes) | Channel-level workarounds for upstream API changes are platform-driven and rarely conflict with the HiveReach restructuring. |
| `tests/test_channels.py` (test additions) | Tests for upstream channel fixes carry the same value here. |
| `hivereach/doctor.py` | Diagnostic logic is shared. |

```bash
git cherry-pick -x <upstream-sha>
```

(`-x` records the source SHA in the commit message — leave that audit trail.)

### ⚠️ Cherry-pick with care

| Path | Why |
|------|-----|
| `hivereach/cli.py` | We've split `_cmd_install` into phases and extracted `_install_helpers`. Most upstream `cli.py` patches will conflict; review and re-apply by hand against the new shape. |
| `hivereach/config.py` | We hardened secret masking and Windows fallback permissions — make sure upstream changes don't regress those. |
| `hivereach/cookie_extract.py` | Largely shared, but the bird/xfetch sync helpers are HiveReach-flavoured — preserve the OSError-only catches. |

### ❌ Do not pull from upstream

| Path | Why |
|------|-----|
| `README.md`, `docs/README_*.md` | Brand and tone diverged. Cherry-picking causes brand churn (Agent-Reach ↔ HiveReach naming). |
| `hivereach/skill/SKILL.md` | Platform count, skill metadata, and routing examples are HiveReach-tuned (16 platforms, our fork URL, our examples). |
| `LICENSE` | Already carries dual attribution. Don't overwrite. |
| `pyproject.toml` (project metadata) | Name, repo URL, author, version are HiveReach-owned. Pull dep additions by hand if needed; never replace the whole file. |
| `CONTRIBUTING.md` | URLs and workflow are HiveReach-specific. |

## Resolving conflicts

When a cherry-pick conflicts:

1. **Channel files**: usually a 3-way merge — keep the upstream API workaround, keep HiveReach's narrowed exceptions and `loguru.debug` logging.
2. **CLI changes**: don't force a merge. Read the upstream patch as a *spec* and re-implement against `_install_phase_*` helpers in `hivereach/cli.py`.
3. **Tests**: prefer running both upstream and existing tests in the same suite — most additions are independent.

After resolving:

```bash
pytest tests/ -v
bash test.sh   # full integration: clean venv → install → doctor
```

## Reference points

- Fork base: `git show fork-base` (= `v1.4.0`)
- Upstream branch: `upstream/main`
- HiveReach trunk: `main`
- Files explicitly diverged: `cli.py`, `config.py`, `cookie_extract.py`, `channels/xueqiu.py`, `_install_helpers.py` (HiveReach-only), all docs.
