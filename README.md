# SSH Agent Auto-Loader

Automatic SSH agent management and key loading for Windows (Git Bash), Linux, and macOS.

<p align="center">
  <img src="https://raw.githubusercontent.com/ZlatanOmerovic/ssh-agent/master/logo-wide.webp" alt="SSH Agent Auto-Loader" width="100%">
</p>

## Features

✅ **Single SSH Agent** - Only one ssh-agent runs regardless of how many terminal sessions you open  
✅ **Auto-Load Keys** - Automatically loads all SSH private keys from `~/.ssh/`  
✅ **Smart Agent Reuse** - Detects and reuses existing agent across sessions  
✅ **Cross-Platform** - Works on Windows (Git Bash), Linux (bash), and macOS (bash/zsh)  
✅ **macOS Keychain Integration** - Optional integration with macOS Keychain for passphrase storage

## Quick Start

### Windows (Git Bash)

```bash
curl -o ~/.bashrc https://raw.githubusercontent.com/ZlatanOmerovic/ssh-agent/master/.bashrc-windows-gitbash
source ~/.bashrc
```

### Linux (bash)

```bash
curl -o ~/.bashrc https://raw.githubusercontent.com/ZlatanOmerovic/ssh-agent/master/.bashrc-linux
source ~/.bashrc
```

### macOS

**For zsh (default since macOS Catalina 10.15):**
```bash
curl -o ~/.zshrc https://raw.githubusercontent.com/ZlatanOmerovic/ssh-agent/master/.zshrc-macos
source ~/.zshrc
```

**For bash:**
```bash
curl -o ~/.bashrc https://raw.githubusercontent.com/ZlatanOmerovic/ssh-agent/master/.bashrc-macos
source ~/.bashrc
```

## Files Included

| File | Purpose |
|------|---------|
| `.bashrc-windows-gitbash` | For Windows Git Bash |
| `.bashrc-linux` | For Linux systems (bash) |
| `.bashrc-macos` | For macOS (bash) - includes Keychain integration |
| `.zshrc-macos` | For macOS (zsh) - **recommended for macOS** |
| `ssh-config-macos` | Optional SSH config for enhanced macOS Keychain integration |

## Detailed Installation

### Prerequisites

1. **SSH directory must exist and have correct permissions:**
   ```bash
   mkdir -p ~/.ssh
   chmod 700 ~/.ssh
   ```

2. **Private keys should have secure permissions:**
   ```bash
   chmod 600 ~/.ssh/id_*
   chmod 600 ~/.ssh/*_rsa
   # Repeat for all your private keys
   ```

### Installation Steps

#### 1. Download the appropriate file for your OS/shell

**Option A: Manual Download**
- Download the appropriate file from this repository
- Rename it to `.bashrc` or `.zshrc` (remove the OS suffix)
- Move it to your home directory (`~/`)

**Option B: Using curl**
```bash
# For Windows Git Bash
curl -o ~/.bashrc https://raw.githubusercontent.com/ZlatanOmerovic/ssh-agent/master/.bashrc-windows-gitbash

# For Linux
curl -o ~/.bashrc https://raw.githubusercontent.com/ZlatanOmerovic/ssh-agent/master/.bashrc-linux

# For macOS with zsh (recommended)
curl -o ~/.zshrc https://raw.githubusercontent.com/ZlatanOmerovic/ssh-agent/master/.zshrc-macos

# For macOS with bash
curl -o ~/.bashrc https://raw.githubusercontent.com/ZlatanOmerovic/ssh-agent/master/.bashrc-macos
```

#### 2. Load the configuration

```bash
# For bash
source ~/.bashrc

# For zsh
source ~/.zshrc
```

Or simply restart your terminal.

#### 3. (Optional) macOS Keychain Integration

For automatic passphrase storage in macOS Keychain:

```bash
# Download the SSH config
curl -o ~/.ssh/config https://raw.githubusercontent.com/ZlatanOmerovic/ssh-agent/master/ssh-config-macos

# Set correct permissions
chmod 600 ~/.ssh/config
```

This enables:
- Automatic passphrase storage in Keychain
- Keys persist across reboots
- No need to re-enter passphrases

## How It Works

### Key Loading Logic

The script automatically loads SSH keys from `~/.ssh/`, **excluding**:
- Files ending in `.pub` (public keys)
- Files ending in `.ppk` (PuTTY format keys)
- `known_hosts` files
- `config` files
- The `agent/` directory

### Agent Management

1. **First terminal session**: Starts a new ssh-agent and loads all keys
2. **Subsequent sessions**: Detects existing agent and reuses it
3. **After reboot**: Starts fresh agent and reloads keys
4. **No duplicates**: Only one ssh-agent process runs at a time

### macOS Keychain Integration

The macOS versions include automatic detection and use of the `--apple-use-keychain` flag:
- **macOS 12.0+**: Uses `--apple-use-keychain` flag
- **Older macOS**: Falls back to standard `ssh-add`
- Stores passphrases securely in macOS Keychain
- Works with both bash and zsh versions

## Verification

After installation, open a new terminal. You should see:

```
Starting new SSH agent...
  Added: id_ed25519
  Added: id_rsa
```

Open another terminal window - it should connect silently (agent already running).

To manually check loaded keys:
```bash
ssh-add -l
```

## Customization

### Show Loaded Keys on Startup

Uncomment these lines at the bottom of your `.bashrc` or `.zshrc`:

```bash
echo "Loaded SSH keys:"
ssh-add -l
```

### Exclude Specific Key Files

Add patterns to the skip conditions in the `load_ssh_keys` function:

```bash
# Add to the existing if statement
if [[ "$key" == *my_special_key* ]]; then
    continue
fi
```

### Load Keys from Additional Directories

Duplicate the for loop and change the path:

```bash
for key in ~/my-other-keys/*; do
    # ... same logic
done
```

## Troubleshooting

### Keys Not Loading

**Check file permissions:**
```bash
ls -la ~/.ssh/
```

Private keys should be `-rw-------` (600).

**Verify keys are valid:**
```bash
ssh-keygen -y -f ~/.ssh/your_key
```

**Check for passphrase issues:**
If your keys have passphrases, you'll be prompted to enter them. On macOS, enable Keychain integration to store passphrases.

### Multiple Agents Running

**Kill all ssh-agent processes:**
```bash
# Linux/macOS
pkill ssh-agent

# Windows Git Bash
taskkill /F /IM ssh-agent.exe
```

**Remove environment file:**
```bash
rm ~/.ssh/agent/environment
```

**Restart terminal.**

### macOS Keychain Not Working

**Ensure SSH config has correct permissions:**
```bash
chmod 600 ~/.ssh/config
```

**Manually add keys to Keychain:**
```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

**Verify Keychain integration:**
```bash
ssh-add -l
```

### Wrong Shell on macOS

**Check your default shell:**
```bash
echo $SHELL
```

- If `/bin/zsh`: Use `.zshrc-macos`
- If `/bin/bash`: Use `.bashrc-macos`

**Change default shell to zsh (recommended):**
```bash
chsh -s /bin/zsh
```

## Compatibility

### Operating Systems
- ✅ Windows 10/11 with Git Bash
- ✅ Linux (all distributions with bash)
- ✅ macOS 10.15 Catalina and later

### Shells
- ✅ bash (all platforms)
- ✅ zsh (macOS, Linux)
- ⚠️ Other shells: May work but not tested

### SSH Versions
- ✅ OpenSSH 7.0+
- ✅ OpenSSH 8.0+ (recommended for macOS Keychain support)

## Security Notes

- Agent environment file is stored with 600 permissions (owner read/write only)
- Private keys should always be 600 permissions
- Public keys can be 644 permissions
- On macOS, Keychain provides encrypted storage for passphrases
- Never commit private keys to version control

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### Reporting Issues

When reporting issues, please include:
- Operating system and version
- Shell type and version (`bash --version` or `zsh --version`)
- SSH version (`ssh -V`)
- Contents of `~/.ssh/` (filenames only, no keys!)
- Error messages

## License

MIT License - Feel free to use and modify as needed.

## Acknowledgments

Inspired by the need for consistent SSH agent management across multiple platforms and terminal sessions.

---

**Star this repo if you find it useful!** ⭐
