# 📦 Repo Tool – Comprehensive Command Guide

[Repo](https://source.android.com/docs/setup/develop/repo) is Google's repository management tool built on top of Git, primarily used for Android development. This guide covers all essential commands with common arguments.

---

## 🔧 Installation & Setup (Linux - Shell)

```bash
# Install repo
mkdir -p ~/bin
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo

# Add to PATH (add to ~/.bashrc or ~/.zshrc)
export PATH="$HOME/bin:$PATH"
```
---

## 🪟 Installation & Setup (Windows - PowerShell)

```powershell
# Create directory for repo
New-Item -ItemType Directory -Path "$HOME\bin" -Force

# Download repo script
Invoke-WebRequest -Uri "https://storage.googleapis.com/git-repo-downloads/repo" -OutFile "$HOME\bin\repo"

# Add to PATH for current session
$env:Path = "$HOME\bin;" + $env:Path

# To persist, add this line to your PowerShell profile ($PROFILE)
'${env:Path} = "$HOME\bin;" + $env:Path' | Out-File -Append -FilePath $PROFILE
```

---

## 🏗️ Project Initialization

### Initialize Repo Client
```bash
repo init -u <manifest-url> [options]
```
**Common Options:**
- `-b <branch>`: Specify manifest branch
- `-m <manifest-file>`: Use specific manifest file
- `--depth=N`: Create shallow clone (history truncated to N commits)
- `--repo-url`: Custom repo repository URL
- `--repo-branch`: Custom repo branch

Example:
```bash
repo init -u https://android.googlesource.com/platform/manifest -b android-14.0.0_r1
```

---

## 🔄 Synchronization Commands

### Basic Sync
```bash
repo sync [<project>...]
```

### Force Sync (DANGER: destroys local changes)
```bash
repo sync --force-sync [<project>...]
```

### Smart Sync Options
```bash
repo sync [options]
```
**Common Options:**
- `-j<N>`: Number of parallel jobs (e.g., `-j8`)
- `-c`: Sync only current branch
- `-d`: Switch projects back to manifest revision
- `-f`: Continue sync even if errors occur
- `--no-tags`: Don't fetch tags
- `--optimized-fetch`: Only fetch when needed

---

## 🌿 Branch Management

### Create New Branch
```bash
repo start <branch-name> [<project>...]
```

### List Branches
```bash
repo branches [<project>...]
```

### Checkout Existing Branch
```bash
repo checkout <branch-name> [<project>...]
```

### Prune Stale Branches
```bash
repo prune [<project>...]
```

---

## 📊 Project Information

### List All Projects
```bash
repo list [options]
```
**Options:**
- `-f`: Show project filesystem paths
- `-n`: Show project names only
- `-p`: Show project paths only

### Show Project Status
```bash
repo status [<project>...]
```
**Useful Options:**
- `-j<N>`: Parallel jobs
- `-o`: Show abbreviated status

### List Manifest File
```bash
repo manifest [options]
```
**Options:**
- `-o <file>`: Output to file
- `--revision-as-HEAD`: Use HEAD for revisions

---

## 🔄 Code Review & Upload

### Upload Changes
```bash
repo upload [options] [<project>...]
```
**Common Options:**
- `--cbr`: Upload current branch only
- `-t`: Include local branch name as topic
- `--no-verify`: Skip pre-upload hooks
- `-y`: Skip confirmation prompt

### Download Change
```bash
repo download <project> <change>
```
Example:
```bash
repo download platform/build 12345/6
```

---

## 🛠️ Advanced Operations

### Rebase Projects
```bash
repo rebase [<project>...] [options]
```
**Options:**
- `-f`: Continue even if conflicts occur
- `-i`: Interactive mode

### Run Git Command Across Projects
```bash
repo forall [<project>...] -c <command>
```
Example:
```bash
repo forall -c 'git clean -xdf'
```

### Diff Across Projects
```bash
repo diff [<project>...]
```

### Show Project Hierarchy
```bash
repo overview
```

---

## ⚙️ Maintenance Commands

### Verify Setup
```bash
repo selfupdate
```

### Run GC on All Projects
```bash
repo gc [<project>...]
```

### Remove Untracked Files
```bash
repo forall -c 'git clean -xdf'
```

---

## 🆘 Getting Help

```bash
repo help
repo help <command>
```

**Useful Help Topics:**
```bash
repo help manifest-format  # Understand manifest files
repo help multi-project    # Working with multiple projects
repo help subcommands     # List all available commands
```

---

## 📚 Additional Resources

- [Official Repo Documentation](https://source.android.com/docs/setup/reference/repo)
- [Manifest Format Guide](https://gerrit.googlesource.com/git-repo/+/master/docs/manifest-format.md)
- [Android Source Setup](https://source.android.com/docs/setup/download)

---

## 📜 License

`repo` is developed and maintained by Google. This guide is community-maintained for developer convenience.
