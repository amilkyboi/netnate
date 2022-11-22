---
draft: false
title: Arch Linux
description: Settings for `archinstall` and `i3-gaps`.

series: [Computing]
catergories: [Linux]
tags: [Arch Linux]

date: 2022-11-14

ShowToc: true
---

## Manual Install

Create an `EFI`, `swap`, and `root` partition. Mount the `EFI` and `root` partitions and enable the swap drive with `swapon`.

## Settings for `archinstall`

- Archinstall language: English
- Keyboard layout: us
- Mirror region: United States
- Locale language: en_US
- Locale encoding: UTF-8
- Drives: 8.0 GB
- Disk layout: Wipe all drives, ext4
- Encryption password: None
- Bootloader: grub-install
- Swap: True
- Hostname: linux-arch-i3wm
- Root password: arch
- User account: username:nathan | password:arch | sudo:true
- Profile: desktop, i3-gaps, VMware/VirtualBox
- Audio: pipewire
- Kernels: linux
- Additional packages: firefox, alacritty
- Network configuration: Copy ISO configuration
- Timezone: US/Central
- NTP: True
- Optional repositories: None

## Settings for `i3-gaps`

Creating xinitrc config file:

- `cp /etc/X11/xinit/xinitrc ~/.xinitrc`
- add `twm &`  (newline)  `exec xrandr --output Virtual1 --mode 1920x1080 --rate 60`

Editing startup procedure, adding bg manager and polybar

1. `sudo pacman -S feh polybar`
2. `vim ~/.config/i3/config`
3. Add three lines:
    1. `exec_always --no-startup-id xrandr --output Virtual1 --mode 1920x1080 --rate 60`
    2. `exec_always --no-startup-id feh --bg-scale ~/Downloads/file-name-background.jpg`
    3. `exec_always --no-startup-id polybar`
        1. Note: the `--no-startup-id` flag is used to prevent startup notification support which eliminates the loading cursor on the i3wm desktop.
4. Add three more lines:
    1. `for_window [class=”.*”] border pixel 0`
    2. `gaps inner 10`
    3. `gaps outer 5`

Editing default terminal [https://wiki.archlinux.org/title/Alacritty](https://wiki.archlinux.org/title/Alacritty):

1. `sudo pacman -S alacritty`
2. `vim ~/.config/i3/config`
3. Change the terminal launch line to `bindsym $mod+Return exec alacritty`

Installing picom [https://wiki.archlinux.org/title/Picom](https://wiki.archlinux.org/title/Picom):

1. `sudo pacman -S picom`
2. `cp /etc/xdg/picom.conf ~/.config/picom/picom.conf`
3. Change `vsync = false;` and `inactive-opacity = 0.80`
4. Add `exec --no-startup-id picom` to `~/.config/i3/config`

Install spotify from the AUR:

1. `sudo pacman -S git`
2. `sudo pacman -S --needed base-devel`
3. `git clone [https://aur.archlinux.org/spotify.git](https://aur.archlinux.org/spotify.git)`
4. `cd spotify`
5. `makepkg -si`
6. `gpg --recv-keys PUBLIC_KEY`
7. `makepkg -si`
8. `spotify -version`
9. `spotify --no-zygote` (for GPU error that is possibly caused in VM’s only.)

Switching to the `sddm` login manager:

1. `sudo pacman -Ru lightdm-gtk-greeter` followed by `sudo pacman -Ru lightdm`
2. Remove the pointer file `sudo rm /whateverpaththefileisin/file.file`
3. `sudo pacman -S sddm`
4. `sudo systemctl enable sddm`
5. `git clone [https://aur.archlinux.org/sddm-sugar-candy-git.git](https://aur.archlinux.org/sddm-sugar-candy-git.git)`
6. `cd sddm-sugar-candy-git`
7. `makepkg -si`
8. `sudo vim /usr/lib/sddm/sddm.conf.d/default.conf`
9. Under `[Theme]` change to `Current=sugar-candy`

Installing custom GRUB theme:

1. Download `Vimix-1080p.tar.xz` from [`https://www.gnome-look.org/p/1009236`](https://www.gnome-look.org/p/1009236)
2. `sudo tar xf Vimix-1080p.tar.xz`
3. `cd Vimix-1080p`
4. `sudo ./install.sh -b`

Installing and switching to `zsh`:

1. List current shell using `neofetch` or `echo $SHELL`
2. `sudo pacman -S zsh`
3. `zsh` and `chsh -l` to verify zsh is installed
4. `chsh -s /usr/bin/zsh`
5. `$mod+Shift+E` to restart i3 and apply changes

Installing `p10k` fonts and `zsh-for-humans`

1. Installing the fonts
    1. First, download the four fonts given [here](https://github.com/romkatv/powerlevel10k#meslo-nerd-font-patched-for-powerlevel10k)
    2. `cd /usr/local/share/fonts/`
    3. `sudo mkdir ttf`
    4. `cd ttf`
    5. `sudo mkdir MesloLGS`
    6. `cd ~/Downloads`
    7. `sudo cp 'MesloLGS NF Regular.ttf' /usr/local/share/fonts/ttf/MesloLGS/` repeat for all four fonts
    8. `cd /usr/local/share/fonts/ttf/MesloLGS`
    9. Change the permissions so that all users can read, write, and execute the files with `sudo chmod a=rwx 'MesloLGS NF Regular.ttf'` repeat for all four fonts
    10. `sudo Xorg :2 -configure` to generate a new config file at `/root/xorg.conf.new`
    11. `sudo cp /root/xorg.conf.new /etc/X11/xorg.conf`
    12. `cd /etc/X11/` and `ls -a` to check for the file
    13. `sudo vim xorg.conf` and find `Section "Files"`
    14. Add `FontPath "/usr/local/share/fonts/ttf/MesloLGS/"`
    15. `cd ~` and `fc-cache` to update fontconfig cache
    16. Change the default font in alacritty
        1. `cd ~/.config/alacritty`
        2. `touch alacritty.yml`
        3. `sudo vim alacritty.yml`
        4. Add the following code

            ```yml
            font:
              normal:
                family: "MesloLGS NF"
            ```

        5. `$mod+Shift+Q` for changes to take effect
2. Installing `zsh-for-humans`
    1. Reload i3 `$mod+Shift+E`
    2. Paste the code below into a shell

        ```bash
        if command -v curl >/dev/null 2>&1; then
          sh -c "$(curl -fsSL https://raw.githubusercontent.com/romkatv/zsh4humans/v5/install)"
        else
          sh -c "$(wget -O- https://raw.githubusercontent.com/romkatv/zsh4humans/v5/install)"
        fi
        ```

    3. `2, 1, n, n` for the initial setup questions
    4. Answer the following graphical questions as they come, everything should work if the fonts are properly installed
    5. 3, 1, 2, 1, 1, 1, 2, 1, 2, 4, 2, 2, 1, n

Improving `vim`

1. Make sure Alacritty is running in true color mode
    1. `cd .config/alacritty/` then `vim alacriltty.yml`
    2. Add the following

        ```yml
        env:
            TERM: xterm-256color
        ```

    3. Test the output using this script from: [https://unix.stackexchange.com/questions/404414/print-true-color-24-bit-test-pattern](https://unix.stackexchange.com/questions/404414/print-true-color-24-bit-test-pattern)

        ```bash
        #!/bin/bash
        # Based on: https://gist.github.com/XVilka/8346728
        
        awk -v term_cols="${width:-$(tput cols || echo 80)}" 'BEGIN{
            s="/\\";
            for (colnum = 0; colnum<term_cols; colnum++) {
                r = 255-(colnum*255/term_cols);
                g = (colnum*510/term_cols);
                b = (colnum*255/term_cols);
                if (g>255) g = 510-g;
                printf "\033[48;2;%d;%d;%dm", r,g,b;
                printf "\033[38;2;%d;%d;%dm", 255-r,255-g,255-b;
                printf "%s\033[0m", substr(s,colnum%2+1,1);
            }
            printf "\n";
        }'
        ```

2. Run the following to install vim-plug

    ```bash
    curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
        https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
    ```

3. `cd ~`, then `touch .vimrc`
4. `sudo vim .vimrc`
5. Add the following code for the `vim-airline` plugin and the `everforest` theme

    ```vimrc
    call plug#begin()
    
    Plug 'vim-airline/vim-airline'
    Plug 'vim-airline/vim-airline-themes'
    Plug 'sainnhe/everforest'
    
    call plug#end()
    
    syntax on
    
    if has('termguicolors')
        set termguicolors
    endif
    
    set background=dark
    let g:everforest_background = 'hard'
    let g:everforest_better_performance = 1
    let g:airline_theme = 'everforest'
    
    colorscheme everforest
    ```

6. `vim` then `:PlugInstall`

Theming `Alacritty`

1. Add the contents of [this](https://gist.github.com/sainnhe/6432f83181c4520ea87b5211fed27950) markdown file to `~/.config/alacritty/alacritty.yml`

Improving `neofetch`

1. Backup the current config `cd ~/.config/neofetch/ && rename config.conf configbackup.conf config.conf`
2. Clone the following `git clone [https://github.com/Chick2D/neofetch-themes/](https://github.com/Chick2D/neofetch-themes/)` into the home directory
3. `mv neofetch-themes/normal/config.conf ~/.config/neofetch/config.conf`

Replacing `htop` with `btm`

1. `sudo pacman -Rs htop`
2. `sudo pacman -S bottom`
3. call with `btm`
4. custom coloring available in `~/.config/bottom.toml`. Coloring should be fine if the Alacritty theme is already set.

Installing `polybar` themes

1. Install the following packages first
    1. `sudo pacman -Syu polybar rofi pywal calc networkmanager_dmenu`
    2. Nerdfont should already be installed from the `p10k` setup
2. `git clone --depth=1 [https://github.com/adi1090x/polybar-themes.git](https://github.com/adi1090x/polybar-themes.git)`
3. `cd polybar-themes`
4. `chmod +x setup.sh`
