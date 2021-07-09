# Raspberry Pi Bootstrap

Personal notes on bootstrapping a basic Raspberry Pi setup

```
# Update the system.
sudo apt-get update
sudo apt-get upgrade  # --yes?

# Install the dependencies
sudo apt-get install \
    emacs \
    mosh \
    zsh

# Make zsh the default shell
chsh -s $(which zsh)

# TODO: Set the hostname
# TODO: Set the ssh-keys
# TODO: Update the password

# Install Tailscale
curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/focal.gpg | sudo apt-key add -
curl -fsSL https://pkgs.tailscale.com/stable/ubuntu/focal.list | sudo tee /etc/apt/sources.list.d/tailscale.list
sudo apt-get update
sudo apt-get install tailscale
sudo tailscale up

Don't forget to disable key expiry if you don't want to have to reauthenticate every time.

# Install GitHub CLI
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo gpg --dearmor -o /usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update
sudo apt install gh

# Generate a new SSH Key
ssh-keygen -t ed25519 -C "hello@jbmorley.co.uk"

# Configure the GitHub user
git config --global user.email "hello@jbmorley.co.uk"
git config --global user.name "Jason Morley"

# Authenticate GitHub
gh auth login

# Install configure-prompt
mkdir -p ~/Projects
cd ~/Projects
git clone git@github.com:jbmorley/configure-prompt.git
echo """FPATH="\$FPATH:\$HOME/Projects/configure-prompt"
autoload configure-prompt
configure-prompt
""" >> ~/.zshrc


# Set the GitHub edtor.
echo "export EDITOR=emacs" >> ~/.zshrc
```
