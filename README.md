# \_dotfiles

Managed with [chezmoi](https://www.chezmoi.io/).

---

## Workflows

### 1. Cloud ahead of local repo

```
  GitHub                 local repo               live config
┌────────┐               ┌────────┐               ┌────────┐
│  NEW   │── git pull ──>│  old   │──── apply ───>│  old   │
└────────┘               └────────┘               └────────┘
```

```sh
chezmoi update          # git pull + apply in one shot
```

or, to preview first:

```sh
chezmoi git pull        # pull only
chezmoi diff            # review what would change
chezmoi apply -v        # apply after review
```

---

### 2. Local repo ahead of cloud

```
  GitHub                 local repo               live config
┌────────┐               ┌────────┐               ┌────────┐
│  old   │<── git push ──│  NEW   │───────────────│   ok   │
└────────┘               └────────┘               └────────┘
```

```sh
chezmoi git -- add -A
chezmoi git -- commit -m "update zed settings"
chezmoi git -- push
```

---

### 3. Local repo changed, live config stale

```
  GitHub                 local repo               live config
┌────────┐               ┌────────┐               ┌────────┐
│   ok   │───────────────│  NEW   │──── apply ───>│  old   │
└────────┘               └────────┘               └────────┘
```

```sh
chezmoi diff            # see what apply would change
chezmoi apply -v        # push source → destination
```

---

### 4. Live config drifted from local repo

```
  GitHub                 local repo               live config
┌────────┐               ┌────────┐               ┌────────┐
│   ok   │───────────────│  old   │<── re-add ────│  NEW   │
└────────┘               └────────┘               └────────┘
```

**Option A — absorb live changes into source:**

```sh
chezmoi status          # see what drifted
chezmoi diff            # inspect the diff
chezmoi re-add          # copy destination → source (overwrites source)
```

**Option B — discard live changes, restore source:**

```sh
chezmoi apply -v        # overwrite destination with source
```

**Option C — 3-way merge:**

```sh
chezmoi merge ~/.config/zed/settings.json   # opens vimdiff
chezmoi merge-all                           # merge everything at once
```

---

## Quick Reference

| Command              | What it does                           |
| -------------------- | -------------------------------------- |
| `chezmoi status`     | Show drift (like `git status`)         |
| `chezmoi diff`       | Show what `apply` would change         |
| `chezmoi apply -v`   | Write source → destination             |
| `chezmoi re-add`     | Write destination → source             |
| `chezmoi update`     | `git pull` + `apply` in one shot       |
| `chezmoi edit FILE`  | Edit source copy, not live file        |
| `chezmoi merge FILE` | 3-way merge (source vs dest vs target) |
| `chezmoi cd`         | `cd` into source dir                   |
| `chezmoi managed`    | List all managed files                 |
| `chezmoi unmanaged`  | List files chezmoi doesn't track       |
