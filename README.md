# Raspberry Pi Bootstrap

Personal notes on bootstrapping a basic Raspberry Pi setup

_These notes are intended for my own personal use only; you are free to use them for reference, but you'll almost certainly find they include steps which don't match your preferred configuration or needs. Please don't use my email address in your own setups 🙃._

## Getting Started

1. Update the installed software.

   ```bash
   sudo apt-get update
   sudo apt-get upgrade --yes
   ```

2. Install some basic dependencies.

   ```
   sudo apt-get install --yes \
       emacs \
       mosh \
       zsh
   ```

3. Make zsh the default shell.

   ```bash
   chsh -s $(which zsh)
   ```
   
4. Customise the zsh prompt (using [`configure-prompt`](https://github.com/jbmorley/configure-prompt)).

   ```bash
   mkdir -p ~/Projects
   cd ~/Projects
   git clone https://github.com/jbmorley/configure-prompt.git
   echo """FPATH="\$FPATH:\$HOME/Projects/configure-prompt"
   autoload configure-prompt
   configure-prompt
   """ >> ~/.zshrc
   ```
   
5. Install [Tailscale](https://tailscale.com).

   ```bash
   curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/focal.gpg | sudo apt-key add -
   curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/focal.list | sudo tee /etc/apt/sources.list.d/tailscale.list
   sudo apt-get update
   sudo apt-get install --yes tailscale
   sudo tailscale up
   ```
   
   You may also wish to disable key expiry for this new machine (see https://login.tailscale.com/admin/machines).
   
6. Generate an SSH key.

   ```bash
   ssh-keygen -t ed25519 -C "hello@jbmorley.co.uk"
   ```

7. Install and authenticate the [GitHub CLI](https://cli.github.com). This is one of the most hands-off ways of adding your SSH key if that's how you prefer to authenticate with git).

   ```bash
   curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo gpg --dearmor -o /usr/share/keyrings/githubcli-archive-keyring.gpg
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
   sudo apt-get update
   sudo apt-get install --yes gh
   gh auth login
   ```
   
8. Configure the default git user.

   ```bash
   git config --global user.email "hello@jbmorley.co.uk"
   git config --global user.name "Jason Morley"
   ```

```
# Set the Git edtor.
echo "export EDITOR=emacs" >> ~/.zshrc
```

## Additional Steps

- [ ] Set the hostname
- [ ] authorized_keys
- [ ] Update the password
