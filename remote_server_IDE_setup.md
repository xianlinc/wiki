# Quick IDE setup in remote server

## Components 

### Main Component
**AstroNvim**

Features:
- Vim
- Aesthetic ✨
- Fuzzy finding files (optional)
- Grep words in all files (optional)
- Code Completion (optional)
- Lazygit (optional)

> [!INFO]
> This guide is meant to be followed in sequential order.
### Simple .bashrc
Add the following lines to your .bashrc
```sh
# prompt color green
export PS1="\[\033[38;5;11m\]\u@\h\[$(tput sgr0)\] \[$(tput sgr0)\]\[\033[38;5;14m\]\w\[$(tput sgr0)\] \[$(tput sgr0)\]\[\033[38;5;4m\]\\$ \[$(tput sgr0)\]"

# vim mode in your command line
set -o vi

# export binary folder to path
export PATH=~/bin:$PATH

alias ll="ls -la --color=auto"
alias la="ls -a --color=auto"
alias ls="ls --color=auto"
```

## Optional extensions
Since we may not have sudo access, we can't use package managers like **apt** or **npm**.
To get around this:
1. We create a directory called bin, which we will put all our executable binaries in.
2. Add it to our path in our .bashrc, so the CLI knows where our executables are.
3. Use curl to download the binaries into ~/bin/storage
4. symlink them into ~/bin

**Fuzzy finding, ripgrep and lazygit** 
- We can curl the nvim, fzf, ripgrep and lazygit zipped folders into ~/bin/storage and symlink their binaries to ~/bin
```bash
mkdir ~/bin
mkdir ~/bin/storage
cd ~/bin/storage
curl -LO https://github.com/neovim/neovim/releases/download/nightly/nvim-linux64.tar.gz
tar xzf nvim-linux64.tar.gz
curl -LO https://github.com/junegunn/fzf/releases/download/0.32.1/fzf-0.32.1-linux_amd64.tar.gz
tar xzf fzf-0.32.1-linux_amd64.tar.gz
curl -LO https://github.com/BurntSushi/ripgrep/releases/download/13.0.0/ripgrep-13.0.0-x86_64-unknown-linux-musl.tar.gz
tar xzf ripgrep-13.0.0-x86_64-unknown-linux-musl.tar.gz
curl -LO https://github.com/jesseduffield/lazygit/releases/download/v0.35/lazygit_0.35_Linux_x86_64.tar.gz
tar xzf lazygit_0.35_Linux_x86_64.tar.gz
cd ~/bin
ln -s -n ~/bin/storage/ripgrep-13.0.0-x86_64-unknown-linux-musl/rg ~/bin/rg
ln -s -n ~/bin/storage/nvim-linux64/bin/nvim ~/bin/nvim
ln -s -n ~/bin/storage/fzf ~/bin/fzf
ln -s -n ~/bin/storage/lazygit ~/bin/lazygit
```

> [!WARNING]
> Make sure you have nvim installed before proceeding

### Setup AstroNvim
In-depth guide: https://astronvim.github.io/

> If you dont have a current nvim config, skip to **clone the repository** 

**Make a backup of current Nvim folder**

```
mv ~/.config/nvim ~/.config/nvim.bak
```

**Clean old plugins (Optional but recommended)[​](https://astronvim.github.io/#clean-old-plugins-optional-but-recommended "Direct link to heading")**

```
mv ~/.local/share/nvim/site ~/.local/share/nvim/site.bak
```

**Clone the repository[​](https://astronvim.github.io/#clone-the-repository "Direct link to heading")**

```
git clone https://github.com/AstroNvim/AstroNvim ~/.config/nvim
nvim +PackerSync
```

#### Installing NVM and node
**Skip this step if you are able to use npm to install packages**
The LSP installer requires npm and node.

We can download [NVM](https://github.com/nvm-sh/nvm), a node version manager that gives us a version of node that is only installed for our user and invoked per-shell.
```bash
cd ~
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
nvm install node
nvm use node
```

You can run `npm -v` to see if everything is installed correctly

> [!WARNING]
> Make sure you can use npm before proceeding

#### Setup LSP and Code completion

Run `:LspInstall <my_lsp>` in neovim. 
> Example of installing python LSP: 
> `:LspInstall pyright` 

**To browse the list of available LSPs:**
- Run `:Mason` in neovim
- You can press the  `?` key to see the help menu in the Mason UI

### Congratulations
You now have a friendly development environment on a remote server!
