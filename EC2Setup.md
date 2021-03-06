# EC2 machine setup

Steps to be followed once the instance has been created. I am assuming that the login would be from WSL (with `~/.ssh` symlinked to `/c/Users/<user>/.ssh` using `ln -s /mnt/c/Users/<user>/.ssh ~/.ssh` in wsl)

1. Create an ssh alias in `~/.ssh/config`
   ```
   Host <short_name_for_remote_host>
     User <ssh_username>
     HostName <remote_host_ip>
     IdentityFile <path_to_key.pem>
   ```

2. Update and upgrade: `sudo apt update -y && apt list --upgradable && sudo apt upgrade -y`

3. Install zsh: `sudo apt install zsh`

4. Install oh-my-zsh:
    ```
    git clone https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
    cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
    edit ZSH_THEME="bira" in the zshrc
    sudo su
    vi /etc/passwd: change ${USER} shell to /bin/zsh instead of /bin/bash

5. Install zsh-syntax-highlighter: 
    ```
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git "$HOME/.zsh-syntax-highlighting" --depth 1
    echo "source $HOME/.zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> "$HOME/.zshrc"
    ```

6. TMux configuration: `cd ~ && wget https://raw.githubusercontent.com/ek-nath/dotfiles/master/.tmux.conf`
'
7. Install TMux plugin manager: `git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm`

8. Install TMux plugins: Launch tmux and hit `I (capital i)

9. Install pyenv: (from: [here](https://github.com/pyenv/pyenv))
   ```
   git clone https://github.com/pyenv/pyenv.git ~/.pyenv
   echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
   echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
   ```
   Logout and log back in to set the variable `pyenv`...
   ```
   echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc
   ```
10. Install pyenv virtualenv: `git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv`
11. Configure pyenv virtualenv to launch: `echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc`
12. Install prerequisites: 
   ```
   sudo apt install -y build-essential libssl-dev zlib1g-dev libbz2-dev \
   libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
   xz-utils tk-dev libffi-dev liblzma-dev python-openssl git
   ```
13. Install python: `pyenv install 3.8.2`
14. Set that version as the global version: `pyenv global 3.8.2`
