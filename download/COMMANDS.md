## Git & GitHub Masterclass
---

## ⚡ QUICK JUMP
| Module | What | Link |
|--------|------|-------|
| M1 | No commands — theory only | — |
| M2 | First push / profile repo | [→ Jump](#module-2) |
| M3 | Project structure + gitignore + uv | [→ Jump](#module-3) |
| M4 | Fork / clone / branch / PR | [→ Jump](#module-4) |
| M5 | Merge conflicts | [→ Jump](#module-5) |
| 🆘 | MISHAP TROUBLESHOOTING | [→ Jump](#mishap-troubleshooting) |

---

---

## MODULE 2
### First Push & GitHub Profile

```bash
# --- 2.1 Git Config (run once per machine) ---
git config --global user.name "Your Full Name"
git config --global user.email "your@email.com"
git config --list                          # Verify — look for user.name and user.email

# --- 2.2 Secret GitHub Feature ---
# On GitHub UI:  + → New Repository → Name = YOUR_EXACT_USERNAME → Public → Add README

# --- 2.3 Local Time Machine ---
mkdir github-profile
cd github-profile
git init                                   # Creates hidden .git/ folder — DO NOT DELETE
echo "# Hi, I'm [Name] 👋" > README.md
nano README.md                             # Or: notepad README.md (Windows)

# --- 2.4 The Big Four ---
git status                                 # GPS — run this constantly
git add .                                  # Stage everything in current folder
git add README.md                          # Stage a specific file
git commit -m "Initial commit: Add profile README"

# Connect local to GitHub (first time only)
git remote add origin https://github.com/vybhav72954/vybhav72954.git
git branch -M main                         # Rename branch to 'main'
git push -u origin main                    # -u sets upstream; after this just: git push

# --- 2.5 Auth troubleshooting (see Mishaps section) ---

# --- 2.6 Verify live ---
# Browser: github.com/vybhav72954
```

---

## MODULE 3
### Professional Project Structure

```bash
# --- 3.1 Context Switch ---
cd ..
mkdir project-1
cd project-1
git init

# --- 3.2 Cookiecutter Structure ---
mkdir data notebooks src models
touch src/__init__.py                      # Makes src/ a Python package
touch notebooks/.gitkeep                   # Placeholder so Git tracks empty folders
touch models/.gitkeep

# --- 3.3 Create .gitignore ---
touch .gitignore
# (Open and paste the contents below)

# --- 3.4 .gitignore contents ---
# data/
# *.csv
# *.parquet
# *.xlsx
# *.h5
# .env
# *.pem
# __pycache__/
# *.pyc
# *.pyo
# .ipynb_checkpoints/
# venv/
# .venv/
# .DS_Store
# Thumbs.db

# --- 3.5 Virtual Environment with uv ---
pip install uv                             # Install uv globally (once)
uv venv                                    # Create .venv/ in current project

# Activate:
source .venv/bin/activate                  # Mac / Linux
source .venv/Scripts/activate              # Windows Git Bash
.venv\Scripts\activate.bat                 # Windows CMD
.venv\Scripts\Activate.ps1                 # Windows PowerShell

# Deactivate when done:
deactivate

# --- 3.6 Grocery List ---
uv pip install pandas numpy yfinance scikit-learn
uv pip freeze > requirements.txt           # Generate the list
uv pip install -r requirements.txt         # Teammate uses this to replicate

# Commit the right files
git add .gitignore requirements.txt src/
git commit -m "feat: Set up project structure with gitignore and requirements"
```

---

## MODULE 4
### Collaboration: Fork → Clone → Branch → PR

```bash
# --- 4.1 Clone ---
cd ..
git clone https://github.com/YOUR-USERNAME/pgdba_git.git
cd pgdba_git

# --- 4.2 Add upstream remote ---
git remote -v                              # See current remotes (only 'origin')
git remote add upstream https://github.com/vybhav72954/pgdba_git.git
git remote -v                              # Now shows 'origin' AND 'upstream'

# --- 4.3 Sync with upstream ---
git fetch upstream                         # Check the mailbox (no code changes yet)
git merge upstream/main                    # Bring the mail inside
# OR combined shortcut:
git pull upstream main                     # Fetch + merge in one

# --- 4.4 Branching ---
git branch                                 # See all local branches (* = current)
git checkout -b feature/add-momentum-signal  # Create AND switch to new branch
git checkout main                          # Switch back to main
git checkout feature/add-momentum-signal   # Switch to feature branch (already exists)

# --- 4.5 Push the Branch ---
git push -u origin feature/add-momentum-signal  # Push to YOUR fork

# --- 4.6 Pull Request ---
# On GitHub UI: Go to your fork → Yellow banner → "Compare & pull request"
# Title: feat: Add momentum signal to feature pipeline
# Description: Explain what + why

# --- 4.7 Auto-close Issues ---
# In PR description body, type:
# Fixes #7
# (When PR is merged, Issue #7 closes automatically)
```

---

## MODULE 5
### Merge Conflicts — Live Demo Setup

```bash
# --- 5.1 SETUP: Create the conflict deliberately ---

# Step 1: Edit a line on main
git checkout main
# (Edit analysis.py line 5 → RSI version)
git add analysis.py
git commit -m "main: Switch to RSI signal"

# Step 2: Edit the SAME line differently on feature branch
git checkout feature/add-momentum-signal
# (Edit analysis.py line 5 → MACD version)
git add analysis.py
git commit -m "feat: Switch to MACD signal"

# Step 3: Attempt to merge (this will FAIL — that's the point)
git checkout main
git merge feature/add-momentum-signal
# OUTPUT: CONFLICT (content): Merge conflict in analysis.py

# --- 5.2 Read the conflict markers ---
cat analysis.py
# You'll see:
# <<<<<<< HEAD
# signal = compute_rsi(prices, window=14)
# =======
# signal = compute_macd(prices, fast=12, slow=26)
# >>>>>>> feature/add-momentum-signal

# --- 5.3 RESOLVE IN PYCHARM ---
# Right-click conflicted file → Git → Resolve Conflicts
# Three-panel view opens:
#   LEFT  = your branch (HEAD/main)
#   RIGHT = incoming branch
#   CENTER = result (what the file will look like)
# Click >> or << arrows to accept a version into center
# OR manually edit center panel for a hybrid
# Click "Apply" when done

# --- 5.4 RESOLVE IN TERMINAL (manual fallback) ---
nano analysis.py                           # Open the file
# DELETE all three marker lines:
#   <<<<<<< HEAD
#   =======
#   >>>>>>> feature/add-momentum-signal
# Keep only the correct version of the code. Save & close.

# Finish the merge
git status                                 # Confirms "All conflicts fixed but still merging"
git add analysis.py
git commit -m "merge: Resolve signal conflict — keep MACD implementation"
git log --oneline                          # Verify merge commit is there
```

---

---

# 🆘 MISHAP TROUBLESHOOTING

> **You're live. Novartis. Stay calm. Every problem below has a clean fix.**

---

### 🪤 TRAP 1 — Stuck in Vim
**Symptom:** You ran `git commit` without `-m`, the screen went black, and there's a cursor blinking inside something called `~`. Nothing types normally.

**What happened:** Git opened the Vim text editor to get your commit message.

**Fix:**
```
Press:  Escape         ← Get to normal mode
Type:   :wq            ← w = write, q = quit
Press:  Enter
```
If that doesn't work:
```
Press:  Escape
Type:   :q!            ← Force quit WITHOUT saving (you'll need to re-commit)
Press:  Enter
```
**Prevention:** Always use `git commit -m "your message"` — never bare `git commit`.

**Permanent fix** (do this now if it happened):
```bash
git config --global core.editor "nano"    # Switch Git's default editor to nano
# OR for VS Code:
git config --global core.editor "code --wait"
```

---

### 🪤 TRAP 2 — Push Rejected: "Updates were rejected"
**Symptom:**
```
! [rejected] main -> main (fetch first)
error: failed to push some refs
```
**What happened:** The remote has commits that your local doesn't have yet. (Someone else pushed, or you committed on GitHub's UI.)

**Fix:**
```bash
git pull origin main --rebase             # Pull and replay your commits on top
git push origin main
```
If there are conflicts after the pull, resolve them (see Module 5), then:
```bash
git rebase --continue
git push origin main
```

---

### 🪤 TRAP 3 — Committed to the Wrong Branch
**Symptom:** You meant to commit to `feature/xyz` but accidentally committed to `main`.

**Fix (the clean surgical approach):**
```bash
# Step 1: Copy the commit ID
git log --oneline
# e.g. output:  a1b2c3d  feat: add momentum signal  ← this is the wrong-branch commit

# Step 2: Switch to the correct branch and cherry-pick the commit
git checkout feature/xyz
git cherry-pick a1b2c3d                   # Apply that exact commit here

# Step 3: Remove it from main (ONLY if main hasn't been pushed yet)
git checkout main
git reset HEAD~1                          # Undo last commit, keep the file changes
```
⚠️ **If `main` was already pushed:** Do NOT use `reset`. Use `git revert HEAD` which creates an undo-commit safely.

---

### 🪤 TRAP 4 — Unstage a File
**Symptom:** You ran `git add .` and accidentally staged your `.env` file or a 5GB CSV.

**Fix:**
```bash
git restore --staged .env                  # Unstage one file (modern Git ≥2.23)
git restore --staged .                     # Unstage EVERYTHING

# Older Git syntax:
git reset HEAD .env
```
The file stays on your disk — it's just removed from the staging area.

---

### 🪤 TRAP 5 — Accidentally Committed Secrets / Large Files
**Symptom:** You committed and pushed `.env` or a big CSV. Now it's on GitHub.

**Immediate triage:**
1. **If the repo is public:** Rotate/invalidate the secret key immediately. GitHub has already indexed it.
2. **Delete the file, commit, push:** This removes it from the *current* version but NOT from history.
3. **To truly remove from history:** Use `git filter-repo` (requires installation) — this is advanced, flag it for after the session.

```bash
# Quick removal (removes from current version):
git rm --cached .env
echo ".env" >> .gitignore
git add .gitignore
git commit -m "fix: Remove accidentally committed secrets"
git push
```

---

### 🪤 TRAP 6 — Abort a Messy Merge
**Symptom:** You started a merge, things are broken everywhere, you want to go back to before the merge started.

**Fix:**
```bash
git merge --abort                         # Instantly resets to pre-merge state
```
This works as long as there are unresolved conflicts (i.e., you haven't committed the merge yet). Safe to run. No data loss.

For a rebase gone wrong:
```bash
git rebase --abort                        # Same idea, different operation
```

---

### 🪤 TRAP 7 — Detached HEAD
**Symptom:** Terminal shows `HEAD detached at a1b2c3d`. Student panics.

**What happened:** They checked out a specific commit or tag directly (e.g., `git checkout a1b2c3d`). They're in a "time travel" state — not on any branch.

**Fix:**
```bash
git checkout main                         # Return to the main branch
# OR if they made commits while detached and want to keep them:
git checkout -b recovery-branch           # Create a branch here first, THEN switch to main
```

---

### 🪤 TRAP 8 — "Nothing to commit, working tree clean" (when they expect changes)
**Symptom:** Student made changes but `git status` shows nothing.

**Causes & fixes:**
```bash
# Cause 1: They're in the wrong directory
pwd                                       # Check where you are
ls                                        # Check what's here
cd correct-project-folder

# Cause 2: The file is gitignored
cat .gitignore                            # Check if the file pattern is listed
git check-ignore -v filename.py           # Shows WHICH gitignore rule is blocking it

# Cause 3: They forgot to save the file in their editor
# → Tell them to save the file (Ctrl+S) and retry
```

---

### 🪤 TRAP 9 — Authentication Failures on Push
**Symptom:** `remote: Support for password authentication was removed` or `403 Forbidden`.

**What happened:** GitHub no longer accepts account passwords via terminal. Needs a PAT or SSH.

**Fix (Personal Access Token — fastest):**
1. GitHub → Profile → Settings → Developer Settings → Personal Access Tokens → Tokens (classic)
2. Generate new token → check `repo` scope → copy the token
3. When terminal asks for password — paste the PAT (it won't show characters)

**Fix (GitHub CLI — cleanest for first-timers):**
```bash
gh auth login                             # Follow the browser prompts
```

**Fix (store credentials so they don't have to re-enter):**
```bash
git config --global credential.helper store   # Stores PAT after first use (Mac/Linux)
git config --global credential.helper manager  # Windows
```

---

### 🪤 TRAP 10 — Branch Already Exists
**Symptom:** `fatal: A branch named 'feature/xyz' already exists`

**Fix:**
```bash
git checkout feature/xyz                  # Just switch to it — don't use -b
# OR to force-recreate it from scratch:
git checkout -b feature/xyz-v2            # Use a different name
```

---

### 🪤 TRAP 11 — `git push` Goes to the Wrong Remote
**Symptom:** Student has both `origin` and `upstream`. Accidentally pushes to upstream (which they don't have write access to → 403 error).

**Fix:**
```bash
git remote -v                             # Check which URL each remote points to
git push origin feature/my-branch        # Always specify the remote explicitly
```

---

### 🪤 TRAP 12 — Merge Conflict Won't Go Away After Editing
**Symptom:** Student edited the file, removed the markers, but `git status` still shows "both modified".

**Fix:**
```bash
# After manually editing and removing ALL <<<<<, =====, >>>>> markers:
git add filename.py                       # You MUST stage the resolved file
git status                                # Should now show "Changes to be committed"
git commit -m "merge: Resolve conflict in filename.py"
```
The student forgot to `git add` after fixing. That's it.

---

### 🪤 TRAP 13 — `git init` in the Home Directory
**Symptom:** Student accidentally ran `git init` in `~` (their home directory) instead of inside the project folder. Now Git is watching their entire machine.

**Fix:**
```bash
cd ~
ls -la                                    # Look for .git/
rm -rf .git                              # DELETE the .git folder from home directory
# Then navigate to the correct folder and re-init:
cd correct-project-folder
git init
```
⚠️ This is one of the most common beginner mistakes. Remind the room: always `cd` into the project folder FIRST, THEN `git init`.

---

### 🆘 NUCLEAR OPTION — Something is Totally Broken
When all else fails during a live demo:

```bash
# See the last 10 things Git did (your audit trail):
git reflog

# Go back to a known-good state using a reflog hash:
git reset --hard HEAD@{3}                 # Go back 3 operations in reflog
```

Or just clone fresh:
```bash
cd ..
git clone https://github.com/vybhav72954/vybhav72954.git github-profile-fresh
cd github-profile-fresh
```

---

## 📋 FULL COMMAND REFERENCE (Alphabetical)

```bash
cat .gitignore                            # View gitignore file contents
deactivate                                # Exit virtual environment
git add .                                 # Stage all changes
git add <file>                            # Stage a specific file
git branch                                # List local branches
git branch -d <branch>                    # Delete a branch (safe — merged only)
git branch -D <branch>                    # Force delete a branch
git cherry-pick <hash>                    # Apply a specific commit to current branch
git checkout <branch>                     # Switch to a branch
git checkout -b <branch>                  # Create and switch to new branch
git clone <url>                           # Download a repo
git commit -m "message"                   # Save a snapshot with a message
git config --global user.email "x@y.com" # Set email globally
git config --global user.name "Name"      # Set name globally
git config --list                         # View all config values
git diff                                  # Show unstaged changes
git diff --staged                         # Show staged changes
git fetch upstream                        # Download upstream changes (no merge)
git init                                  # Start tracking a folder with Git
git log --oneline                         # View commit history (compact)
git merge <branch>                        # Merge a branch into current branch
git merge --abort                         # Cancel a merge mid-conflict
git pull origin main                      # Fetch + merge from origin/main
git pull upstream main                    # Fetch + merge from upstream/main
git push                                  # Push to set upstream
git push -u origin main                   # Push + set upstream (first time)
git push origin <branch>                  # Push a specific branch
git rebase --abort                        # Cancel a rebase
git reflog                                # Audit trail of all Git operations
git remote -v                             # List all remotes
git remote add origin <url>               # Add origin remote
git remote add upstream <url>             # Add upstream remote
git reset HEAD~1                          # Undo last commit (keep changes)
git reset --hard HEAD~1                   # Undo last commit (DISCARD changes)
git restore --staged <file>               # Unstage a file
git revert HEAD                           # Undo last commit by creating a new commit
git rm --cached <file>                    # Untrack a file without deleting it
git stash                                 # Temporarily shelve uncommitted changes
git stash pop                             # Re-apply stashed changes
git status                                # Show current state (run constantly)
source .venv/bin/activate                 # Activate venv (Mac/Linux)
source .venv/Scripts/activate             # Activate venv (Windows Git Bash)
uv pip freeze > requirements.txt          # Generate requirements file
uv pip install -r requirements.txt        # Install from requirements file
uv venv                                   # Create virtual environment
```
