---
name: ghq
description: Efficiently manage and discover Git repositories using ghq. Use this skill when the user mentions working with multiple repositories, searching for projects, cloning new repositories, or navigating between different codebases. Trigger whenever repository discovery, cross-repository operations, or project exploration is needed.
---

# ghq: Efficient Repository Management

## What is ghq?

ghq is a repository management tool that organizes Git repositories in a structured directory layout. All repositories are stored under a common root directory (typically `~/ghq`), organized by hostname and path.

**Example structure:**
```
~/ghq/
├── github.com/
│   ├── octocat/
│   │   ├── skills/
│   │   └── dotfiles/
│   └── someorg/
│       └── somerepo/
└── gitlab.com/
    └── user/
        └── project/
```

## Core Commands

### `ghq root`
Shows the root directory where all repositories are stored.

```bash
ghq root
# Output: /Users/username/ghq
```

Use this to:
- Determine the base path for all repositories
- Build full paths to specific repositories
- Understand where repositories will be cloned

### `ghq list`
Lists all repositories managed by ghq.

```bash
ghq list
# Output:
# github.com/octocat/skills
# github.com/octocat/dotfiles
# github.com/someorg/somerepo
```

Options:
- `ghq list -p` : Show full paths instead of relative paths
- `ghq list --full-path` : Same as `-p`

Use this to:
- Discover available repositories
- Search for repositories by name or path
- Verify a repository exists before working with it
- Get the exact path to a repository

### `ghq get <repository>`
Clones a repository and places it in the correct location under ghq root.

```bash
ghq get https://github.com/user/repo
# or
ghq get github.com/user/repo
# or
ghq get user/repo  # assumes github.com
```

The repository will be automatically placed at:
`$(ghq root)/github.com/user/repo`

## When to Use ghq

### 1. Discovering Repositories

**Instead of manually searching directories:**
```bash
# ❌ Don't do this
find ~ -name "project-name" -type d
```

**Use ghq list with filtering:**
```bash
# ✅ Do this
ghq list project-name
# or get full paths
ghq list -p | grep project-name
```

### 2. Cloning New Repositories

**Instead of git clone:**
```bash
# ❌ Don't do this
git clone https://github.com/user/repo
cd repo
```

**Use ghq get:**
```bash
# ✅ Do this
ghq get https://github.com/user/repo
cd "$(ghq list -p github.com/user/repo | head -n1)"
```

Why this is better:
- Consistent directory structure
- No need to decide where to clone
- Easy to find later with `ghq list`
- Integrates with other ghq workflows

### 3. Working Across Multiple Repositories

When the user asks to work across multiple repositories:

1. **First, discover repositories:**
   ```bash
   ghq list | grep <search-term>
   ```

2. **Get full paths:**
   ```bash
   ghq list -p | grep <search-term>
   ```

3. **Work with each repository:**
   ```bash
   for repo in $(ghq list -p | grep <pattern>); do
     cd "$repo"
     # perform operations
   done
   ```

**Example workflow:**
```bash
# Find all repos belonging to an organization
ghq list | grep "github.com/orgname"

# Update all of them
for repo in $(ghq list -p | grep "github.com/orgname"); do
  cd "$repo"
  git pull
done
```

### 4. Checking if a Repository Exists

Before attempting to work with a repository, verify it exists:

```bash
if ghq list | grep -q "github.com/user/repo"; then
  cd "$(ghq root)/github.com/user/repo"
  # do work
else
  echo "Repository not found. Clone with: ghq get github.com/user/repo"
fi
```

## Best Practices

### Always prefer ghq commands

- **For repository discovery:** Use `ghq list`, not `find` or `ls`
- **For cloning:** Use `ghq get`, not `git clone`
- **For path resolution:** Use `ghq root` + `ghq list -p`, not hardcoded paths

### Build paths programmatically

Instead of hardcoding paths:
```bash
# ❌ Don't do this
cd ~/ghq/github.com/user/repo
```

Do this:
```bash
# ✅ Do this
cd "$(ghq root)/github.com/user/repo"
# or
REPO_PATH=$(ghq list -p | grep user/repo | head -n1)
cd "$REPO_PATH"
```

### Handle multiple matches gracefully

When `ghq list | grep` returns multiple results, clarify with the user:

```bash
repos=$(ghq list | grep "pattern")
count=$(echo "$repos" | wc -l)

if [ "$count" -gt 1 ]; then
  echo "Multiple repositories found:"
  echo "$repos"
  # Ask user to be more specific
fi
```

## Common Patterns

### Pattern 1: Find and navigate to a repository

```bash
# Search for the repository
REPO=$(ghq list -p | grep "project-name" | head -n1)

# Navigate to it
if [ -n "$REPO" ]; then
  cd "$REPO"
else
  echo "Repository not found"
fi
```

### Pattern 2: Clone if not exists, then navigate

```bash
REPO_NAME="github.com/user/repo"

if ! ghq list | grep -q "$REPO_NAME"; then
  ghq get "$REPO_NAME"
fi

cd "$(ghq root)/$REPO_NAME"
```

### Pattern 3: Batch operations across repositories

```bash
# Find all React projects (example)
for repo in $(ghq list -p); do
  if [ -f "$repo/package.json" ] && grep -q "react" "$repo/package.json"; then
    echo "Processing: $repo"
    cd "$repo"
    # perform operations
  fi
done
```

## Integration with Other Tools

ghq works well with:

- **fzf**: `cd $(ghq list -p | fzf)`
- **peco**: `cd $(ghq list -p | peco)`
- **Shell aliases**: Users may have custom aliases that wrap ghq commands

If the user mentions using these tools, respect their workflow.

## Error Handling

### Repository not found
If `ghq list` doesn't return a repository the user mentions:
1. Confirm the repository name with the user
2. Offer to clone it: `ghq get <repo-url>`

### Multiple ghq roots (edge case)
Some users configure multiple ghq roots. If needed, you can check:
```bash
git config --get-all ghq.root
```

But typically, `ghq root` returns the primary root.

## Summary

The key mindset shift: **Think of repositories as managed by ghq, not as random directories.**

- Don't search the filesystem → use `ghq list`
- Don't `git clone` anywhere → use `ghq get`
- Don't hardcode paths → use `ghq root` + `ghq list -p`

This approach keeps repository management consistent, discoverable, and efficient.
