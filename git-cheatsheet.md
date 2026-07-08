# Git Cheatsheet
**Curtis Tucker** | github.com/c2tucker

---

## First-Time Setup
```bash
git config --global user.name "Your Name"        # Set your name
git config --global user.email "you@email.com"   # Set your email
git config --global init.defaultBranch main       # Set default branch to main
git config --global --list                        # Verify all settings
```

---

## Starting a Repo
```bash
git init                                          # Initialize a new local repo
git clone git@github.com:user/repo.git            # Clone a repo via SSH
git clone https://github.com/user/repo.git        # Clone a repo via HTTPS
```

---

## The Core Daily Loop
```bash
git status                                        # What has changed?
git add filename.sh                               # Stage a specific file
git add .                                         # Stage all changes
git commit -m "Describe what changed"             # Save a snapshot
git push                                          # Send to GitHub
```

---

## Checking Status and History
```bash
git status                                        # Show staged, unstaged, untracked files
git log                                           # Full commit history
git log --oneline                                 # Compact commit history
git log --oneline --graph                         # Visual branch history
git diff                                          # Show unstaged changes (line by line)
git diff --staged                                 # Show staged changes before committing
```

---

## Staging and Unstaging
```bash
git add filename.sh                               # Stage a specific file
git add .                                         # Stage everything
git add -p                                        # Stage changes interactively (chunk by chunk)
git restore --staged filename.sh                  # Unstage a file (keep changes)
git restore filename.sh                           # Discard unstaged changes to a file (permanent)
```

---

## Committing
```bash
git commit -m "Add backup script"                 # Commit with inline message
git commit --amend -m "Fixed message"             # Fix the last commit message (before pushing)
git commit --amend --no-edit                      # Add forgotten file to last commit
```

> **Commit message convention:** Use present tense, imperative mood.
> "Add backup script" not "Added backup script"

---

## Branches
```bash
git branch                                        # List local branches (* = current)
git branch -a                                     # List all branches including remote
git checkout -b feature-name                      # Create and switch to new branch
git switch -c feature-name                        # Same (newer syntax)
git checkout main                                 # Switch to existing branch
git switch main                                   # Same (newer syntax)
git merge feature-name                            # Merge branch into current branch
git branch -d feature-name                        # Delete a branch (after merging)
git branch -D feature-name                        # Force delete a branch
```

> **Branch workflow:** Always switch to `main` first, then merge into it.

---

## Working with GitHub (Remote)
```bash
git remote add origin git@github.com:user/repo.git   # Connect local repo to GitHub
git remote -v                                         # Show connected remotes
git push -u origin main                               # First push (sets upstream)
git push                                              # All subsequent pushes
git pull                                              # Download and merge remote changes
git fetch                                             # Download remote changes (don't merge yet)
```

> **Rule of thumb:** Always `git pull` before you start working to avoid conflicts.

---

## Undoing Things
```bash
git restore filename.sh                           # Discard unstaged changes (permanent)
git restore --staged filename.sh                  # Unstage a file (changes kept)
git revert HEAD                                   # Undo last commit by creating a new commit
git reset --soft HEAD~1                           # Undo last commit, keep changes staged
git reset --hard HEAD~1                           # Undo last commit, discard all changes (destructive)
```

> **Safe vs destructive:**
> `restore` and `revert` are safe — they don't rewrite history.
> `reset --hard` is destructive — changes are gone permanently.

---

## .gitignore
Create a `.gitignore` file in your repo root to tell Git what to never track.

```bash
nano .gitignore                                   # Create/edit the file
```

**Common patterns:**
```
.env                    # Secrets and API keys — NEVER commit these
*.log                   # Log files
__pycache__/            # Python cache directory
*.pyc                   # Compiled Python files
node_modules/           # Node.js dependencies
.DS_Store               # macOS metadata (auto-generated)
Scratch/                # Local scratch/temp directories
*.pem                   # Private key files
```

> **Best practice:** Create `.gitignore` before your first commit. Much harder to un-track files after they've been committed.

---

## SSH and GitHub Authentication
```bash
ssh-keygen -t ed25519 -C "label"                 # Generate a new SSH key pair
ssh-add ~/.ssh/id_ed25519                         # Load key into SSH agent
ssh-add -l                                        # List loaded keys
pbcopy < ~/.ssh/id_ed25519.pub                    # Copy public key to clipboard (Mac)
ssh -T git@github.com                             # Test GitHub connection
cat ~/.ssh/config                                 # View SSH config
```

**~/.ssh/config block for GitHub:**
```
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519
```

---

## Stashing (Saving Work Temporarily)
```bash
git stash                                         # Stash current uncommitted changes
git stash list                                    # List all stashes
git stash pop                                     # Restore most recent stash
git stash drop                                    # Delete most recent stash
```

> Useful when you need to switch branches but aren't ready to commit.

---

## Viewing and Comparing
```bash
git show <commit-hash>                            # Show details of a specific commit
git diff main..feature-name                       # Compare two branches
git blame filename.sh                             # Show who changed each line and when
```

---

## Repo Structure Best Practices
```
~/Git/
├── bash-scripts/       # Polished, documented scripts
├── homelab/            # Lab documentation and infrastructure
├── linux-practice/     # Daily reps and experiments
├── notes/              # Cheat sheets and reference material
├── python-practice/    # Python learning and scripts
└── sandbox/            # Throwaway experiments
```

---

## The Three Zones of Git
```
Working Directory  →  git add  →  Staging Area  →  git commit  →  Local Repo  →  git push  →  GitHub
     (your files)                  (the box)                      (snapshot)                  (remote)
```

---

## Quick Reference: Most Used Commands
| Command | What it does |
|---|---|
| `git status` | Your constant orientation tool |
| `git add .` | Stage all changes |
| `git commit -m "..."` | Save a snapshot |
| `git push` | Send to GitHub |
| `git pull` | Get latest from GitHub |
| `git log --oneline` | Compact history |
| `git diff` | See what changed |
| `git restore filename` | Discard changes to a file |

---

*Last updated: July 2026*
