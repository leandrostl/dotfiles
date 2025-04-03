# Configuring the environment
## Setting oh-my-zsh

1. From this [documentation](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH), lets start installing ZSH:

```bash
# Install Zsh from apt repository
   sudo apt install zsh
# Set as default shell
   chsh -s $(which zsh)
```

1. Logout to get finish configure default options
1. [Install oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh):

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## Synchronizing the dotfiles

1. **Clone the dotfiles** repo as **bare**:

   Since we want to use the `$HOME` directory without overwriting it, cloning as **bare** allows you to clone without a working directory, creating only a _.git_ file, or in our case, a _.cfg_ file.

   ```bash
   # Clone the repository as a bare repository into the specified directory, which only gets the .git file
   git clone --bare <git-repo-url> $HOME/.cfg
   ```

2. **Create an alias** for the `config` command:

   As you can see, we are registering a new command called `config`. It defines where the _git_ configuration is and what comprises the work tree, i.e., what git should look for. You should put this in the `.bashrc` file or any other shell configuration file. If you do so, you should apply `source .bashrc` to reload the configurations.

   ```bash
   alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
   ```

3. **Create a backup** of existing config files:

   The command below will backup every file that has already been tracked by the dotfiles repo. This backup will be put in a `.config-backup` folder and can be restored.

   ```bash
   # Create a backup directory
   mkdir -p .config-backup && \
   config checkout 2>&1 | egrep "\s+\." | awk {'print $1'} | \
   xargs -I{} mv {} .config-backup/{}
   ```

4. **Check out** the synchronized files:

   After backing up the existing configuration files, you can check out the dotfiles from the repository. As you can see, we are already using `config` command.

   ```bash
   config checkout
   ```

5. **Prevent untracked files from being listed** with the `config` command:

   This command configures the local repository to not show untracked files in the status output.

   ```bash
   config config --local status.showUntrackedFiles no
   ```

6. Now you can version configuration files with commands like:
   ```shell
   config status
   config add .vimrc
   config commit -m "Add vimrc"
   config add .bashrc
   config commit -m "Add bashrc"
   config push
   ```
## Setting Zsh Autosuggestion
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

## Setting Agnoster theme to oh-my-zsh

After set up dotfiles configurations, you will see some strange characters on Zsh. It's because ubuntu doesn't have powerline fonts defined by default. So install that!
```bash 
sudo apt install fonts-powerline
```
Installed the font package, open the terminal and change configuration to accept a custom font and choose `Ubuntu Mono`

## Setting Audio Switcher

```shell
sudo apt-add-repository ppa:yktooo/ppa
sudo apt update
sudo apt install indicator-sound-switcher
```

More details on [Sound Switcher Indicator](https://yktoo.com/en/software/sound-switcher-indicator/)
