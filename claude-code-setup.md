# Developer Environment Setup Guide: Antigravity, Claude Code, Git & GitHub CLI

This guide walks you through installing Google Antigravity, Claude Code, Git, and the GitHub CLI, then integrating them for a powerful AI-assisted development workflow.

---

## 1. Installing Google Antigravity

Google Antigravity is an agent-first IDE built on VS Code, powered by Gemini 3 Pro. It enables autonomous AI agents to plan, execute, and verify complex development tasks.

### System Requirements

| Platform | Requirements |
|----------|-------------|
| macOS | 10.15 (Catalina) or later, Intel or Apple Silicon |
| Windows | Windows 10 or 11, 64-bit processor |
| All | 8GB RAM minimum (16GB recommended), 2GB disk space |

### macOS Installation

1. **Download the installer** from [antigravity.google/download](https://antigravity.google/download)
   - Choose "Download for Apple Silicon" or "Download for Intel" based on your Mac
2. **Install the application**
   - Open the downloaded `.dmg` file
   - Drag the Antigravity icon to your Applications folder
3. **First launch**
   - Open Antigravity from Applications
   - Sign in with your Google Account (personal Gmail accounts work; Workspace accounts have limited support during preview)
   - Grant requested permissions for file system access and network connections
4. **Complete setup wizard**
   - Choose whether to import VS Code/Cursor settings or start fresh
   - Select your preferred theme (dark/light)
   - Configure your preferred AI model (Gemini 3 Pro is default)

### Windows Installation

1. **Download the installer** from [antigravity.google/download](https://antigravity.google/download)
   - Select "Download for x64" (most common) or "Download for ARM64"
2. **Run the installation wizard**
   - Double-click the downloaded `.exe` file
   - Follow the installation prompts (use default settings)
   - Sign in with your Google Account
3. **First launch**
   - Launch Antigravity
   - Enable "Tab Gitignore Access" (bottom-right "Antigravity - Settings" -> "Tab Gitignore Access")

---

## 2. Installing Claude Code

Claude Code is Anthropic's terminal-based AI coding assistant that understands your entire codebase and helps with code generation, debugging, and git workflows.

### Prerequisites

- **Subscription required:** Claude Pro, Claude Max, Claude for Teams/Enterprise, or an Anthropic Console account with billing enabled
- **Operating Systems:** macOS 13.0+, or Windows 10+ (with WSL, WSL 2, or Git Bash)

### Native Installation (Recommended)

The native binary installation is recommended as it avoids package manager conflicts and auto-updates.

Open your terminal application:
- **macOS**: Press `Command + Space`, type "Terminal", and press `Enter`.
- **Windows**: Press the `Windows` key, type "PowerShell", and press `Enter`.

Copy and paste the command then run:

**macOS:**

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**Windows (PowerShell):**

```powershell
irm https://claude.ai/install.ps1 | iex
```

**macOS via Homebrew:**

```bash
brew install claude-code
```

Note: Homebrew installations don't auto-update. Run `brew upgrade claude-code` periodically.

**Windows via WinGet:**

```powershell
winget install Anthropic.ClaudeCode
```

### Verify Installation

In your terminal:

```bash
# Check installation
claude doctor

# Check version
claude --version
```

### First Run and Authentication

In your terminal, create a temporary folder and navigate to it 

```bash
# Navigate to your project
cd /path/to/your/temporary/folder

# Start Claude Code
claude
```

On first run, Claude Code opens a browser window for authentication. Sign in with your Claude subscription or Anthropic Console account.

### Fixing "command not found" Errors

If `claude` isn't recognized after installation, add the binary path to your shell:

**Bash:**
```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

**Windows (PowerShell)**
```powershell
$env:PATH = "$home/.local/bin;$PATH" | Out-File -Append -FilePath $PROFILE
```

---

## 3. Installing Git

Git is essential for version control and integrates deeply with both Antigravity and Claude Code.

### macOS

**Option 1: Xcode Command Line Tools (simplest)**
```bash
xcode-select --install
```

**Option 2: Homebrew**
```bash
brew install git
```

### Windows

**Option 1: Git for Windows (recommended)**
1. Download from [git-scm.com/download/win](https://git-scm.com/download/win)
2. Run the installer with default options
3. This includes Git Bash, which Claude Code can use

**Option 2: WinGet**
```powershell
winget install Git.Git
```

### Post-Installation Configuration

```bash
# Set your identity
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Set default branch name
git config --global init.defaultBranch main

# Enable helpful colors
git config --global color.ui auto

# Verify installation
git --version
```

---

## 4. Installing GitHub CLI (gh)

The GitHub CLI lets you interact with GitHub from the terminal — creating PRs, managing issues, and more.

### macOS

```bash
brew install gh
```

### Windows

**WinGet:**
```powershell
winget install GitHub.cli
```

**Scoop:**
```powershell
scoop install gh
```

**Chocolatey:**
```powershell
choco install gh
```

### Authentication

```bash
# Authenticate with GitHub
gh auth login

# Follow the prompts:
# - Choose GitHub.com or GitHub Enterprise
# - Choose HTTPS or SSH
# - Authenticate via browser or paste a token
```

### Verify Installation

```bash
gh --version
gh auth status
```

---

## 5. Integrating Claude Code and Git with Antigravity

### Install Claude Code Extension

In your terminal:

```bash
antigravity --install-extension anthropic.claude-code
```

### Running Claude Code Inside Antigravity's Terminal

Antigravity includes an integrated terminal (inherited from VS Code). You can run Claude Code directly within it:

1. **Open Antigravity** and navigate to your project
2. **Launch Claude Code via Extension** Click the Claude Code * icon on the left-hand sidebar of Antigravity
3. **Alternatively: Launch Claude Code via Terminal** Open the terminal** with `` Ctrl+' ``
4. **Launch Claude Code:**
   ```bash
   claude
   ```
5. **Use both agents in parallel:**
   - Use Antigravity's Gemini agents for high-level planning (via the Agent panel, `Cmd/Ctrl+L`)
   - Use Claude Code in the terminal for precise code execution
   - This lets you leverage Gemini's planning capabilities and Claude's coding precision together

## 6. Configure Git for Both Tools

Both Antigravity and Claude Code work seamlessly with Git. Ensure your Git configuration is consistent:

```bash
# Optional: Set Antigravity as your default editor
git config --global core.editor "antigravity --wait"
```

### Install Github Actions Extension

In your terminal:

``` bash
antigravity --install-extension GitHub.vscode-github-actions
```

Claude Code can handle Git operations directly through natural language:
- "Commit these changes with message 'Add authentication'"
- "Create a new branch called feature/user-profile"
- "Show me the diff for the last commit"

### Hybrid Workflow (Planning + Execution)

A powerful pattern is using Antigravity for planning and Claude Code for execution:

1. **In Antigravity's Agent Manager:**
   - Describe your feature: "I need to add user authentication with OAuth"
   - Let Gemini create a detailed implementation plan and architecture

2. **Export the plan** (Antigravity creates Markdown artifacts)

3. **In Claude Code:**
   - Share the plan: "Here's the implementation plan. Let's start with step 1."
   - Claude Code executes each step with precision

4. **Use Git throughout:**
   ```bash
   # Claude Code can handle this
   "Create a feature branch, implement the auth module, and commit"
   ```

### Recommended Workflow Summary

| Task | Tool | Why |
|------|------|-----|
| Project planning & architecture | Antigravity Agent | Multi-agent orchestration, visual artifacts |
| Code implementation | Claude Code | Precise code generation, terminal integration |
| Quick inline edits | Antigravity Editor | Familiar VS Code experience |
| Git operations | Either | Both handle Git well; Claude Code excels at conversational git |
| Testing & debugging | Both + MCP | Connect testing services via MCP |
| PR creation & review | gh CLI + Claude Code | "Create a PR for this branch with a summary" |

---

## Quick Reference: Verification Commands

```bash
# Check all installations
antigravity --version    # Or launch the app
claude --version
git --version
gh --version

# Verify Claude Code setup
claude doctor

# Verify GitHub authentication
gh auth status

# Verify Git configuration
git config --list
```

---

## Troubleshooting

### Antigravity won't launch
- Ensure you're signed in with a personal Gmail account (Workspace has limited support)
- Check system requirements (especially RAM)
- Try reinstalling from antigravity.google/download

### Claude Code "command not found"
- Add `~/.local/bin` to your PATH (see section 2)
- Run `claude doctor` to diagnose issues
- Try reinstalling with the native installer

### Git authentication issues
- Use `gh auth login` to refresh GitHub credentials
- Check SSH keys with `ssh -T git@github.com`
