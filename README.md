# Configurando para sincronizar dotfiles

1. Clone o repo de dotfiles como *bare*: 
    ```bash
    git clone --bare <git-repo-url> $HOME/.cfg
    ```
2. Cria um alias para config:
    ```bash
    alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
    ```
3. Faça um backup dos arquivos de configuração anterio
   ```bash
   mkdir -p .config-backup && \
   config checkout 2>&1 | egrep "\s+\." | awk {'print $1'} | \
   xargs -I{} mv {} .config-backup/{}
   ```
1. Faça o checkout dos arquivos sincronizados
   ```bash
   config checkout
   ```
1. Evite que arquivos não trackeados sejam listados com o comando config
   ```bash
   config config --local status.showUntrackedFiles no
   ```
1. Agora pode versionar arquivos de configurações com comandos como:
   ```bash
   config status
   config add .vimrc
   config commit -m "Add vimrc"
   config add .bashrc
   config commit -m "Add bashrc"
   config push
   ```