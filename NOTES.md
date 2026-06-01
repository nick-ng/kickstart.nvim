We've downgraded to nvim v0.11.5. To revert to the version maintained by brew,
remove the symlink and remove the files.

```
rm ~/.local/bin/nvim && rm -rf ~/.local/opt/nvim-0.11.5/
```

Claude Code output:

```
❯ how did you revert to an older version of nvim?

⏺ Homebrew only ships the latest neovim, so we sidestepped homebrew entirely:

  1. Downloaded the prebuilt binary from neovim's GitHub releases:
  curl -L https://github.com/neovim/neovim/releases/download/v0.11.5/nvim-macos-arm64.tar.gz -o
  /tmp/nvim-0.11.5.tar.gz
  tar -xzf /tmp/nvim-0.11.5.tar.gz -C /tmp
  1. This gives you a self-contained nvim-macos-arm64/ directory with bin/, lib/, and share/. The share/
  directory has nvim's runtime files (color schemes, default ftplugins, etc.) — you need to keep the
  directory structure intact, not just copy out the binary.
  2. Stripped macOS quarantine so Gatekeeper wouldn't block it:
  xattr -dr com.apple.quarantine /tmp/nvim-macos-arm64/
  3. Moved it to a stable location instead of leaving it in /tmp:
  mkdir -p ~/.local/opt
  mv /tmp/nvim-macos-arm64 ~/.local/opt/nvim-0.11.5
  4. Put it earlier in PATH by symlinking into ~/.local/bin/, which your $PATH already ranks higher than
  /opt/homebrew/bin/:
  ln -sf ~/.local/opt/nvim-0.11.5/bin/nvim ~/.local/bin/nvim

  After that, which nvim resolves to ~/.local/bin/nvim and runs 0.11.5. The homebrew 0.12.0 binary is
  still installed at /opt/homebrew/bin/nvim, just shadowed — so future brew upgrade neovim etc. won't
  fight you, and removing the symlink instantly switches back.

  To undo: rm ~/.local/bin/nvim && rm -rf ~/.local/opt/nvim-0.11.5/.
```
