# Configuring the environment
## Synchronizing the dotfiles
1. **Clone the dotfiles** repo as **bare**:

   Since we want to use the `$HOME` directory without overwriting it, cloning as **bare** allows you to clone without a working directory, creating only a *.git* file, or in our case, a *.cfg* file.
    ```bash
   # Clone the repository as a bare repository into the specified directory, which only gets the .git file
   git clone --bare <git-repo-url> $HOME/.cfg
    ```
2. **Create an alias** for the `config` command:

   As you can see, we are registering a new command called `config`. It defines where the *git* configuration is and what comprises the work tree, i.e., what git should look for. You should put this in the `.bashrc` file or any other shell configuration file. If you do so, you should apply `source .bashrc` to reload the configurations.
   
    ```bash
    alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
    ```
3. **Create a backup** of existing config files:

   The command below will backup every file that has already been tracked by the dotfiles repo. This backup will be put in a `.config-backup` folder and can be restored.
   
   ```bash
   # Create a backup directory
   mkdir -p .config-backup && \
   # Attempt to check out the dotfiles, capture any files that would be overwritten
   config checkout 2>&1 | egrep "\s+\." | awk {'print $1'} | \
   # Move each of these files to the backup directory
   xargs -I{} mv {} .config-backup/{}
   ```
1. **Check out** the synchronized files:

   After backing up the existing configuration files, you can check out the dotfiles from the repository. As you can see, we are already using `config` command.
   ```bash
   config checkout
   ```
1. **Prevent untracked files from being listed** with the `config` command:

   This command configures the local repository to not show untracked files in the status output.
   ```bash
   config config --local status.showUntrackedFiles no
   ```
1. Now you can version configuration files with commands like:
   ```shell
   config status
   config add .vimrc
   config commit -m "Add vimrc"
   config add .bashrc
   config commit -m "Add bashrc"
   config push
   ```
## Setting Zsh

## Setting Audio Switcher

```shell
sudo apt-add-repository ppa:yktooo/ppa
sudo apt update
sudo apt install indicator-sound-switcher
```
More details on [Sound Switcher Indicator](https://yktoo.com/en/software/sound-switcher-indicator/)