# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a macOS setup automation project using Ansible and Homebrew. The main purpose is to provision a new Mac with applications, system preferences, and dotfiles through a single command: `./install`.

## Core Architecture

### Bootstrap Process (install script)
The main entry point is the `./install` bash script which orchestrates a 4-phase setup:
1. **Bootstrap Phase**: Installs Xcode Command Line Tools, Homebrew, and Ansible
2. **Package Installation**: Runs `brew bundle` using the `Brewfile`
3. **System Configuration**: Executes the Ansible playbook (`playbook.yml`)
4. **Final Steps**: Restarts system services to apply changes

### Ansible Structure
The project uses Ansible with a localhost inventory and 4 main roles:
- `homebrew`: Package management and Brewfile execution
- `macos-defaults`: System preferences configuration using `defaults write`
- `dotfiles`: Shell configuration and dotfiles from Jinja2 templates
- `applications`: Application-specific configurations

## Key Commands

### Main Installation
```bash
# Complete setup (recommended for fresh Mac)
./install
```

### Selective Execution
```bash
# Run only specific Ansible roles using tags
ansible-playbook playbook.yml -i inventory.yml --tags "homebrew"
ansible-playbook playbook.yml -i inventory.yml --tags "macos"
ansible-playbook playbook.yml -i inventory.yml --tags "dotfiles"
ansible-playbook playbook.yml -i inventory.yml --tags "apps"

# Interactive execution (step through tasks)
ansible-playbook playbook.yml -i inventory.yml --step

# Package management only
brew bundle --file=Brewfile
```

### Development Commands
```bash
# Syntax check Ansible playbook
ansible-playbook --syntax-check playbook.yml

# Dry run without making changes
ansible-playbook playbook.yml -i inventory.yml --check

# List all available tags
ansible-playbook playbook.yml -i inventory.yml --list-tags
```

## Configuration Structure

### Package Management
- `Brewfile`: Homebrew packages and cask applications
- `vars/packages.yml`: Structured package definitions (minimal usage)

### System Preferences
- `vars/defaults.yml`: macOS system preferences organized by category:
  - `dock_settings`: Dock appearance and behavior
  - `finder_settings`: Finder preferences and view options
  - `global_ui_settings`: Keyboard, mouse, and UI behavior
  - `security_settings`: Screen lock and security preferences
  - `trackpad_settings`: Trackpad behavior configuration
  - `screenshot_settings`: Screenshot format and location

### Dotfiles and Shell Configuration
- `roles/dotfiles/templates/`: Jinja2 templates for dotfiles
  - `zshrc.j2`: Zsh shell configuration with Oh My Zsh
  - `gitconfig.j2`: Git global configuration
  - `vimrc.j2`: Vim/Neovim configuration
  - `tmux.conf.j2`: Terminal multiplexer configuration
- `roles/dotfiles/defaults/main.yml`: Personal git credentials and shell preferences

## Customization Patterns

### Adding New Packages
1. Edit `Brewfile` for Homebrew packages:
   ```ruby
   brew "new-cli-tool"
   cask "new-application"
   ```
2. Add custom taps if needed:
   ```ruby
   tap "custom/tap"
   brew "custom/tap/package"
   ```

### Modifying System Preferences
1. Add entries to appropriate section in `vars/defaults.yml`:
   ```yaml
   dock_settings:
     - { key: "new-setting", type: "bool", value: true }
   ```

### Updating Dotfiles
1. Modify Jinja2 templates in `roles/dotfiles/templates/`
2. Variables available from `roles/dotfiles/defaults/main.yml`

### Personal Configuration
Update `roles/dotfiles/defaults/main.yml` with your information:
```yaml
git_user_name: "Your Name"
git_user_email: "your.email@example.com"
```

## Automated Package Management Workflow

This project includes a Claude Code hook (`.claude.json`) that automatically enforces the following workflow when adding packages:

### Standard Package Addition Process
When you ask Claude to add packages, it will automatically:
1. **Add packages to Brewfile** - Update the Brewfile with new packages and required taps
2. **Run ./install** - Execute the full installation script to install packages and apply configurations
3. **Update documentation** - Modify both README.md and CLAUDE.md to reflect the new packages
4. **Commit and push changes** - Always create a git commit and push to remote after making changes

### Hook Configuration
The `.claude.json` file contains a `UserPromptSubmit` hook that detects package-related requests and automatically reminds Claude to follow the complete workflow. This ensures consistency and prevents missed steps.

### Manual Override
If you need to bypass the automated workflow:
- Edit the Brewfile directly and run specific commands manually
- Use `brew bundle --file=Brewfile` to install only packages without full system configuration
- Disable the hook temporarily by renaming `.claude.json`

## Git Workflow Requirements

**IMPORTANT**: After making ANY changes to this repository, always:
1. **Commit changes** with a descriptive message
2. **Push to remote** to keep the repository up-to-date

This applies to:
- Package additions or removals
- Configuration changes (macOS defaults, dotfiles, etc.)
- Documentation updates
- Any file modifications

### Standard Commit Process
```bash
git add .
git commit -m "Descriptive commit message"
git push origin main
```

The automated workflow will handle this for package-related changes, but for all other modifications, explicitly commit and push the changes immediately after making them.

## Important Implementation Details

- The project is idempotent - can be run multiple times safely
- System services (Dock, SystemUIServer, Finder) are restarted post-configuration
- Xcode Command Line Tools installation includes wait loop for completion
- Apple Silicon Macs get special Homebrew PATH handling in the install script
- All system preferences use proper `defaults write` commands with appropriate domains
- Dotfiles are backed up before replacement (configurable via `backup_dotfiles`)
- The playbook includes error handling with `ignore_errors` where appropriate