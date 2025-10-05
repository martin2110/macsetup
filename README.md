# macOS Setup Automation

A complete macOS setup automation using Ansible and Homebrew. Run one command to set up a new Mac with all your essential applications, configurations, and dotfiles.

## Quick Start

```bash
git clone https://github.com/yourusername/macsetup.git
cd macsetup
./install
```

That's it! The script will:
1. Install Xcode Command Line Tools
2. Install Homebrew
3. Install Ansible
4. Install packages from Brewfile
5. Run Ansible playbook to configure system preferences
6. Set up dotfiles and shell configuration

## What Gets Installed

### Applications
- **Google Chrome** - Web browser
- **iTerm2** - Terminal replacement
- **Visual Studio Code** - Code editor
- **Docker Desktop** - Containerization platform
- **1Password** - Password manager
- **Obsidian** - Knowledge management and note-taking
- **Slack** - Team communication and collaboration
- **Wireshark** - Network protocol analyzer

### CLI Tools
- **git** - Version control
- **wget** - File downloader
- **fzf** - Fuzzy finder
- **neovim** - Modern Vim-based text editor
- **ollama** - Run large language models locally
- **jq** - JSON processor and query tool
- **tree** - Directory structure visualization
- **htop** - Interactive process viewer
- **bat** - Cat with syntax highlighting
- **go** - Go programming language
- **node** - Node.js JavaScript runtime
- **python@3.12** - Python programming language
- **gawk** - GNU AWK text processing
- **mtr** - Network diagnostic tool (traceroute + ping)
- **pyenv** - Python version management
- **tmux** - Terminal multiplexer
- **gitkraken-cli** - GitKraken command line interface
- **helm** - Kubernetes package manager
- **gh** - GitHub CLI for repository management
- **act** - Run GitHub Actions locally
- **go-task** - Task runner / build tool
- **civo** - Civo Cloud CLI
- **yq** - YAML processor and query tool
- **kubectl** - Kubernetes command-line tool
- **kluctl** - GitOps deployment tool
- **nmap** - Network exploration and security auditing tool

### Fonts
- **Hack Nerd Font** - Programming font with icons
- **Intone Mono Nerd Font** - Monospace programming font with icons

## What Gets Configured

### Complete macOS Configuration Changes

#### 🎛️ Dock Settings (`com.apple.dock`)
- **Tile Size**: 50px (medium size icons)
- **Auto-hide**: Enabled (dock hides when not in use)
- **Position**: Bottom of screen
- **Recent Apps**: Disabled (no recent applications shown)
- **Minimize to Application**: Enabled (windows minimize into app icon)
- **Magnification**: Enabled with 60px large size

#### 📁 Finder Settings (`com.apple.finder`)
- **Show Hidden Files**: Enabled (`.files` and system files visible)
- **Status Bar**: Shown at bottom of Finder windows
- **Path Bar**: Shown at bottom of Finder windows
- **Default View**: List view (`Nlsv`)
- **Search Scope**: Current folder (`SCcf`)
- **Library Folder**: Unhidden (`chflags nohidden ~/Library`) - **Requires Password**

#### ⌨️ Keyboard & Input Settings (`NSGlobalDomain`)
- **Key Repeat Rate**: 2ms (very fast)
- **Initial Key Repeat**: 15ms (short delay before repeat starts)
- **Full Keyboard Access**: Enabled (tab through all controls)
- **Press and Hold**: Disabled (allows key repeat instead of accent menu)
- **Auto-corrections**: All disabled
  - Automatic capitalization: Off
  - Smart dashes: Off
  - Smart periods: Off
  - Smart quotes: Off
  - Spell check: Off

#### 🖱️ Trackpad Settings
- **Tap to Click**: Enabled for trackpad (`com.apple.driver.AppleBluetoothMultitouch.trackpad`)
- **Tap to Click**: Enabled for mouse/trackpad globally (`NSGlobalDomain`)

#### 🔒 Security & Privacy Settings
- **Screen Lock**: Disabled (no password required after screensaver) (`com.apple.screensaver`)
- **Password Delay**: Disabled (0 seconds)
- **Screen Idle Time**: 10 minutes (600 seconds)

#### 📸 Screenshot Configuration (`com.apple.screencapture`)
- **Format**: PNG (high quality)
- **Location**: `~/Desktop/Screenshots` (organized storage)
- **Shadows**: Disabled (clean screenshots)

#### 🕐 Menu Bar Settings
- **Clock Format**: Full date and time (`EEE MMM d  h:mm:ss a`)
  - Shows: "Mon Oct 3  2:30:45 PM"
- **Menu Bar Clock**: Always visible

#### 📄 File & Save Dialog Settings (`NSGlobalDomain`)
- **File Extensions**: Always show all file extensions
- **Save Dialogs**: Always expanded by default
- **Save Mode**: Expanded state remembered

#### 🐚 Shell & Terminal Configuration
- **Default Shell**: Changed to Zsh (`/bin/zsh`) - **Requires Password**
- **Oh My Zsh**: Installed with bullet-train theme
- **Shell Aliases**: Enhanced commands (ls, grep, etc.)
- **History**: Improved settings with search and deduplication

#### 💻 Application-Specific Configurations

##### Visual Studio Code (`~/Library/Application Support/Code/User/settings.json`)
- **Font**: Hack Nerd Font, 14px
- **Tab Size**: 2 spaces
- **Editor**: Auto-save on focus change, format on save
- **Rulers**: 80 and 120 character guides
- **Theme**: Default Dark+
- **Terminal Font**: Hack Nerd Font
- **Git**: Smart commit enabled, sync confirmation disabled
- **Whitespace**: Boundary rendering enabled

##### iTerm2 (`com.googlecode.iterm2`)
- **Font**: IntoneMono Nerd Font, 18pt
- **Color Scheme**: 0x96f (dark theme)
  - Background: Dark gray (#0F0F0F)
  - Foreground: Light gray (#999999)
  - Cursor: Orange accent (#F26010)
  - Bold text: Bright gray (#CCCCCC)

##### Startup Applications (Login Items)
- **Docker Desktop**: Auto-start on login
- **1Password**: Auto-start on login (if installed)

##### Git Configuration (`~/.gitconfig`)
- Global user name and email (from variables)
- Global gitignore file (`~/.gitignore_global`)
- Sensible defaults for push, pull, and merge behavior

##### Vim/Neovim Configuration
- Basic configuration with useful settings
- Syntax highlighting and line numbers
- Modern defaults for editing

##### Tmux Configuration (`~/.tmux.conf`)
- Sensible defaults for terminal multiplexing
- Enhanced key bindings
- Status bar configuration

##### Python Development Environment
- **pyenv**: Python version management
- **Default Python**: Latest stable version installed
- **Virtual Environment**: Default venv created in `~/.venv/`

### Password Requirements Summary
The following operations require your macOS password:
1. **Shell Change** (`chsh -s /bin/zsh`) - Changes default shell to Zsh
2. **Library Folder** (`chflags nohidden ~/Library`) - Unhides the Library folder

All other configurations use the `defaults write` command which modifies user preferences without requiring admin privileges.

## Customization

### Modify Packages
Edit `Brewfile` to add or remove packages:
```ruby
# Add new applications
cask "new-app"

# Add new CLI tools
brew "new-tool"
```

### Modify System Preferences
Edit `vars/defaults.yml` to customize macOS settings:
```yaml
dock_settings:
  - { key: "tilesize", type: "int", value: 48 }  # Bigger dock
```

### Modify Dotfiles
Edit templates in `roles/dotfiles/templates/`:
- `zshrc.j2` - Shell configuration
- `gitconfig.j2` - Git configuration
- `vimrc.j2` - Vim configuration

### Git Configuration
Before running, you may want to set your git details in `roles/dotfiles/defaults/main.yml`:
```yaml
git_user_name: "Your Name"
git_user_email: "your.email@example.com"
```

## Running Specific Parts

Run only specific roles using tags:
```bash
# Only install packages
ansible-playbook playbook.yml -i inventory.yml --tags "homebrew"

# Only configure system preferences
ansible-playbook playbook.yml -i inventory.yml --tags "macos"

# Only set up dotfiles
ansible-playbook playbook.yml -i inventory.yml --tags "dotfiles"
```

## Manual Steps After Installation

1. **Sign into applications** that require authentication
2. **Set up SSH keys** for Git repositories
3. **Configure 1Password** browser integration
4. **Import settings** for applications that support it
5. **Restart terminal** or source shell configuration

## Troubleshooting

### Homebrew Installation Issues
If Homebrew fails to install, run:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Ansible Issues
If Ansible tasks fail, try running them individually:
```bash
ansible-playbook playbook.yml -i inventory.yml --step
```

### Permission Issues
Some system preferences changes require full disk access. Grant permission when prompted.

## File Structure

```
macsetup/
├── install                    # Main installation script
├── Brewfile                   # Homebrew packages and apps
├── ansible.cfg               # Ansible configuration
├── playbook.yml              # Main Ansible playbook
├── inventory.yml             # Ansible inventory
├── requirements.yml          # Ansible requirements
├── vars/
│   ├── packages.yml          # Package definitions
│   └── defaults.yml          # System preference settings
├── roles/
│   ├── homebrew/            # Package management
│   ├── macos-defaults/      # System preferences
│   ├── dotfiles/            # Shell and editor configs
│   └── applications/        # App-specific settings
└── README.md                # This file
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test on a fresh macOS installation
5. Submit a pull request

## License

MIT License - feel free to use and modify as needed.

## Acknowledgments

- [Homebrew](https://brew.sh/) for package management
- [Ansible](https://ansible.com/) for configuration management
- [Oh My Zsh](https://ohmyz.sh/) for shell enhancement

## Installation Summary

When you run `./install`, this automation will:

### Phase 1: Bootstrap
- Install Xcode Command Line Tools (if not present)
- Install Homebrew package manager (if not present)
- Install Ansible automation tool (if not present)

### Phase 2: Package Installation
- **Applications**: Google Chrome, iTerm2, Visual Studio Code, Docker Desktop, 1Password, Obsidian, Slack, Wireshark (network protocol analyzer)
- **CLI Tools**: git, wget, fzf (fuzzy finder), neovim (modern vim), ollama (local LLMs), jq (JSON processor), tree (directory visualization), htop (process viewer), bat (enhanced cat), go (programming language), node (JavaScript runtime), python@3.12 (Python), gawk (text processing), mtr (network diagnostics), pyenv (Python versions), tmux (terminal multiplexer), gitkraken-cli (git interface), helm (Kubernetes package manager), gh (GitHub CLI), act (GitHub Actions locally), go-task (task runner), civo (Civo Cloud CLI), yq (YAML processor), kubectl (Kubernetes CLI), kluctl (GitOps deployment), nmap (network security auditing)
- **Fonts**: Hack Nerd Font with programming icons

### Phase 3: System Configuration
- **Dock**: Auto-hide, 36px size, bottom position, hide recent apps
- **Finder**: Show hidden files, status bar, path bar, list view, search current folder
- **Global UI**: Show all file extensions, full keyboard access, disable press-and-hold
- **Security**: Require password immediately after screensaver
- **Trackpad**: Enable tap-to-click functionality
- **Keyboard**: Fast key repeat (2ms) with short initial delay (15ms)
- **Screenshots**: PNG format, saved to ~/Desktop/Screenshots, no shadows
- **Menu Bar**: Show clock with full date/time format

### Phase 4: Dotfiles & Shell Setup
- **Zsh**: Set as default shell with Oh My Zsh framework
- **Git**: Global configuration with sensible defaults and gitignore
- **Vim/Neovim**: Basic configuration with useful settings
- **Tmux**: Terminal multiplexer with sensible defaults
- **Shell Aliases**: Useful shortcuts and enhanced commands

### Phase 5: Application Settings
- **VS Code**: Font configuration (requires first launch to create settings directory)
- **Development Environment**: Ready for coding with proper terminal, editor, and git setup

The entire process is idempotent - you can run it multiple times safely, and it will only make changes when needed.