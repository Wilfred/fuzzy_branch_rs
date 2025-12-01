# git-fuzzy

A Rust implementation of [FuzzyBranch](https://github.com/Wilfred/FuzzyBranch) - a git tool that simplifies branch checkout by allowing partial branch name matching.

## Features

Instead of typing the complete branch name:
```bash
$ git checkout feature/really-long-branch-name
```

You can use just enough characters to uniquely identify it:
```bash
$ git co real
```

The tool supports:
- **Exact matching**: If the input exactly matches a branch name
- **Substring matching**: If the input is contained in one or more branch names
- **Silent operation**: No output on successful single match (only git's own output)
- **Colored highlighting**: Matched substrings are highlighted in green when showing ambiguous matches
- **Commit checkout**: Falls back to checking out commits if no branch matches
- **Smart branch tracking**: Shows local branches and remote-only branches without duplicates

## Installation

1. Build the project:
```bash
cargo build --release
```

2. Copy the executable to a directory in your `$PATH`:
```bash
cp target/release/git-fuzzy ~/.local/bin/
# or
cp target/release/git-fuzzy /usr/local/bin/
```

3. Add a git alias to your `~/.gitconfig`:
```ini
[alias]
    co = !git-fuzzy
```

## Usage

After installation, use the `git co` alias (or run `git-fuzzy` directly):

```bash
# Get help
$ git-fuzzy --help
$ git-fuzzy --version

# Checkout a branch with substring matching (silent on success)
$ git co dev
Switched to branch 'develop'

# Checkout with partial name (silent on success)
$ git co real
Switched to branch 'feature/really-long-branch-name'

# Ambiguous matches show all options with highlighted match
$ git co feat
Ambiguous branch name 'feat'. Multiple matches:
  feature/another-feature        # 'feat' is highlighted in green
  feature/really-long-branch-name

# Fall back to commit checkout if no branch matches
$ git co 620a729
No branches match '620a729', trying as commit...
HEAD is now at 620a729 initial commit
```

## How It Works

1. **Branch Discovery**: Retrieves all local branches and remote branches using `git for-each-ref`
2. **Smart Filtering**: Shows local branches plus remote-only branches (avoiding duplicates)
3. **Fuzzy Matching**:
   - First tries exact match
   - Falls back to substring match if no exact match found
   - If no branch matches, attempts to checkout as a commit
4. **Checkout**: Executes `git checkout` with the matched branch or commit

## Requirements

- Rust 1.70 or later (for building)
- Git (for running)

## License

This is a Rust reimplementation of the original Haskell [FuzzyBranch](https://github.com/Wilfred/FuzzyBranch) by Wilfred Hughes.
