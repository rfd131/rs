# Fork workflow for rfd131/rs

This fork carries our own Runestone customizations (for the biocalc PreTeXt
book) while still sending selected changes upstream. This file lives **only on
the `biocalc` branch** — never merge it toward `main` or a PR branch.

## Branches

| Branch | Role | Rules |
|---|---|---|
| `main` | Pristine mirror of `upstream/main` | Never commit here. Only `merge --ff-only upstream/main`. |
| `biocalc` | Our integration branch: upstream + all our changes | What we build/deploy from. Only receives merges. |
| feature branches | One per change, always branched from `main` | Merge into `biocalc` right away; optionally PR to upstream. |

Branching every feature from clean `main` (never from `biocalc`) is what keeps
upstream PRs free of our private customizations.

## Making a change

```bash
git fetch upstream
git checkout -b my-change upstream/main   # always from clean upstream
# ... work, commit ...
git push -u origin my-change

# use it ourselves immediately:
git checkout biocalc && git merge --no-ff my-change && git push

# if (and only if) it's a candidate for upstream:
gh pr create --repo RunestoneInteractive/rs --base main --head rfd131:my-change
```

Local-only changes follow the same steps minus the `gh pr create`.

## Syncing with upstream (do this periodically and after our PRs merge)

```bash
git fetch upstream
git checkout main && git merge --ff-only upstream/main && git push
git checkout biocalc && git merge main && git push
```

Conflicts during the `biocalc` merge usually mean upstream touched something
we customized. If upstream merged our own PR (possibly with edits), prefer
upstream's version of those hunks so we converge with them.

## Seeing what's ours

```bash
git log --oneline --first-parent main..biocalc   # our merges on top of upstream
git diff main...biocalc --stat                    # net content difference
```
