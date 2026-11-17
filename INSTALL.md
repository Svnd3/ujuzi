# Install

One copy-paste block for Ubuntu. It downloads this repo, finds the `skills/` folder, and installs every skill into your agent's skills directory.

**Repo owner:** replace `OWNER/REPO` in the block below with your GitHub path (for example `jane/universal-skills`) before publishing. Then users only need to copy and paste — no edits.

---

## Quick start (Ubuntu)

Copy everything in this block, paste it into a terminal, press Enter.

```bash
(
set -euo pipefail

# Change OWNER/REPO here (or leave it and paste the URL when prompted).
REPO="${REPO:-https://github.com/OWNER/REPO}"
BRANCH="${BRANCH:-main}"
REPO="${REPO%.git}"

if [[ "$REPO" == *OWNER/REPO* ]]; then
  printf 'Paste your GitHub repo URL: '
  read -r REPO_INPUT
  [ -n "${REPO_INPUT:-}" ] || { echo "No repo URL given. Aborting."; exit 1; }
  REPO="${REPO_INPUT%.git}"
fi

need() { command -v "$1" >/dev/null 2>&1; }
need curl || need wget || { echo "Missing curl/wget. Run: sudo apt install -y curl"; exit 1; }
need tar || { echo "Missing tar. Run: sudo apt install -y tar"; exit 1; }

tmp="$(mktemp -d)"
trap 'rm -rf "$tmp"' EXIT

url="${REPO%/}/archive/refs/heads/${BRANCH}.tar.gz"
printf 'Downloading %s\n' "$url"
if need curl; then
  curl -fL --retry 3 --connect-timeout 15 "$url" -o "$tmp/repo.tar.gz" \
    || { echo "Download failed. Check the repo URL and branch name."; exit 1; }
else
  wget -q -O "$tmp/repo.tar.gz" "$url" \
    || { echo "Download failed. Check the repo URL and branch name."; exit 1; }
fi

tar -xzf "$tmp/repo.tar.gz" -C "$tmp"
src="$(find "$tmp" -maxdepth 3 -type d -name skills -print -quit)"
[ -n "$src" ] || { echo "Could not find a skills/ folder in the download."; exit 1; }

if [ -n "${SKILLS_DIR:-}" ]; then
  targets=("$SKILLS_DIR")
else
  targets=("$HOME/.claude/skills" "$HOME/.codex/skills")
fi

total=0
for target in "${targets[@]}"; do
  mkdir -p "$target"
  count=0
  for dir in "$src"/*/; do
    [ -d "$dir" ] || continue
    [ -f "$dir/SKILL.md" ] || continue
    name="$(basename "$dir")"
    if [ -e "$target/$name" ]; then
      mv "$target/$name" "$target/$name.bak.$(date +%s)"
    fi
    cp -R "$dir" "$target/$name"
    count=$((count + 1))
  done
  printf 'Installed %d skill(s) into %s\n' "$count" "$target"
  total=$((total + count))
done

printf '\nInstalled skills:\n'
for dir in "$src"/*/; do
  [ -f "$dir/SKILL.md" ] || continue
  printf '  - %s\n' "$(basename "$dir")"
done

printf '\nFinished: %d install(s). Restart your agent to pick up the skills.\n' "$total"
)
```

### What it does

1. Downloads the repo tarball (no `git` required).
2. Finds the `skills/` folder.
3. Installs each skill folder into `~/.claude/skills` and `~/.codex/skills`.
4. Backs up any existing folder of the same name to `<name>.bak.<timestamp>` instead of deleting it.
5. Cleans up the temporary download.

No `sudo` is needed. Nothing outside your home directory is touched.

---

## Install to a custom location

Set `SKILLS_DIR` and run the same block. This skips the default two directories and installs only where you point it.

```bash
export SKILLS_DIR="$HOME/my-agent/skills"
```

Then paste the Quick start block. To make it permanent for one command without exporting, prefix it:

```bash
SKILLS_DIR="$HOME/my-agent/skills" bash
```

*(Inside that shell, paste the Quick start block, then type `exit`.)*

---

## Install from a local folder (test before publishing)

Use this while the repo is still on your machine. `cd` into the folder that contains `skills/`, then run:

```bash
cd "$HOME/Downloads/UNIVERSAL SKILLS" && (
set -euo pipefail

targets=("$HOME/.claude/skills" "$HOME/.codex/skills")
[ -n "${SKILLS_DIR:-}" ] && targets=("$SKILLS_DIR")
src="$PWD/skills"
[ -d "$src" ] || { echo "Run this from the folder that contains skills/"; exit 1; }

for target in "${targets[@]}"; do
  mkdir -p "$target"
  count=0
  for dir in "$src"/*/; do
    [ -d "$dir" ] || continue
    [ -f "$dir/SKILL.md" ] || continue
    name="$(basename "$dir")"
    if [ -e "$target/$name" ]; then
      mv "$target/$name" "$target/$name.bak.$(date +%s)"
    fi
    cp -R "$dir" "$target/$name"
    count=$((count + 1))
  done
  printf 'Installed %d skill(s) into %s\n' "$count" "$target"
done

printf '\nDone.\n'
)
```

If your folder is somewhere else, change the `cd` path on the first line.

---

## Verify

```bash
ls -1 "$HOME/.claude/skills"
```

You should see 12 folders, including `audit`, `design-qa`, `get-context`, and `react-best-practices`. Each contains a `SKILL.md`.

Check one file:

```bash
head -n 5 "$HOME/.claude/skills/audit/SKILL.md"
```

---

## Update

Re-run the Quick start block. Existing folders are moved to a timestamped backup, then replaced with the latest version.

To roll back, rename a backup over the current folder:

```bash
mv "$HOME/.claude/skills/audit.bak.1730000000" "$HOME/.claude/skills/audit"
```

---

## Uninstall

```bash
for name in audit accessibility-and-inclusive-visualization design-qa frontend-app-builder frontend-testing-debugging get-context ideate image-to-code react-best-practices research url-to-code visualization-strategy-and-critique; do
  rm -rf "$HOME/.claude/skills/$name" "$HOME/.codex/skills/$name"
done
```

Backups (`*.bak.*`) are left untouched. Remove them separately if you want them gone:

```bash
rm -rf "$HOME"/.claude/skills/*.bak.* "$HOME"/.codex/skills/*.bak.*
```

---

## Troubleshooting

| Problem | Fix |
| --- | --- |
| `curl: (22) ... 404` | The repo path or branch is wrong. Confirm `OWNER/REPO` and that the default branch is `main` (otherwise run with `BRANCH=master`). |
| `Could not find a skills/ folder` | The repo layout changed. Make sure `skills/` sits at the repo root. |
| Skills not appearing in the agent | Restart the agent. Confirm the skills folder exists and contains `<name>/SKILL.md`. |
| Permission denied | Do not use `sudo`. Check ownership of `~/.claude` or `~/.codex` with `ls -ld`. |
| Want to keep an existing skill | Rename it before installing, or restore it from the `.bak.<timestamp>` folder afterward. |
