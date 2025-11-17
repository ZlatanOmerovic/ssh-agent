# Contributing to SSH Agent Auto-Loader

Thank you for considering contributing to this project! 🎉

## How to Contribute

### Reporting Bugs

When reporting bugs, please include:

1. **Operating System & Version**
   - Windows: `winver` output
   - Linux: `uname -a` and distro info
   - macOS: `sw_vers` output

2. **Shell Information**
   ```bash
   echo $SHELL
   bash --version  # or zsh --version
   ```

3. **SSH Version**
   ```bash
   ssh -V
   ```

4. **Contents of ~/.ssh/** (filenames only, never include actual keys!)
   ```bash
   ls -la ~/.ssh/
   ```

5. **Error Messages**
   - Copy the full error output
   - Include any relevant terminal output

6. **Steps to Reproduce**
   - Clear, numbered steps
   - What you expected vs what happened

### Suggesting Enhancements

- Open an issue describing the enhancement
- Explain why it would be useful
- Provide examples if possible

### Pull Requests

1. **Fork the repository**

2. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make your changes**
   - Follow existing code style
   - Keep functions small and focused
   - Add comments for complex logic

4. **Test your changes**
   - Test on the relevant platform(s)
   - Ensure no regressions
   - Test with multiple terminal sessions
   - Test with and without existing ssh-agent

5. **Commit with clear messages**
   ```bash
   git commit -m "Add feature: brief description"
   ```

6. **Push and create PR**
   ```bash
   git push origin feature/your-feature-name
   ```

## Code Style Guidelines

### Shell Script Style

- Use 4 spaces for indentation (not tabs)
- Functions should have descriptive names
- Add comments for non-obvious logic
- Use `local` for function variables
- Quote variables: `"$variable"` not `$variable`
- Use `[[` instead of `[` for conditionals

### Good Example:
```bash
function load_ssh_keys {
    # Add all private keys (excluding .pub and .ppk files)
    local keys_loaded=0
    
    for key in ~/.ssh/*; do
        # Skip if it's a directory
        if [ -d "$key" ]; then
            continue
        fi
        
        # Process the key...
    done
}
```

### Documentation

- Update README.md if adding features
- Add inline comments for complex code
- Update CHANGES.md for significant changes
- Keep documentation accurate and up-to-date

## Testing Checklist

Before submitting a PR, verify:

- [ ] Works on target platform(s)
- [ ] No syntax errors (`shellcheck` if available)
- [ ] Handles edge cases (no keys, locked keys, etc.)
- [ ] Doesn't break existing functionality
- [ ] Documentation updated if needed
- [ ] Commit messages are clear

## Platform-Specific Considerations

### Windows (Git Bash)
- Test with different Git for Windows versions
- Be aware of path format differences
- Test with Windows Defender enabled

### Linux
- Test on at least one major distro (Ubuntu/Fedora/Arch)
- Consider systemd vs init systems
- Test with different terminal emulators

### macOS
- Test on both Intel and Apple Silicon if possible
- Test with both bash and zsh
- Verify Keychain integration works
- Consider macOS version differences

## Questions?

Feel free to open an issue for:
- Clarification on contribution guidelines
- Questions about implementation
- Ideas you want to discuss before coding

## Code of Conduct

- Be respectful and inclusive
- Focus on constructive feedback
- Help others learn and grow
- Assume good intentions

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

**Thank you for making this project better!** 🙏
