# Jetbrains

## Java

```bash
yay -S jdk-openjdk openjdk-doc # latest
yay -S jdkX-openjdk openjdkX-doc # version X
```





## Idea

```bash
sudo pacman -S intellij-idea-ultimate-edition intellij-idea-ultimate-edition-jre
```





### Fix `fish: Unable to open universal variable file '...': Permission denied`

```bash
sudo ln -s ~/.config/fish/fish_variables /opt/intellij-idea-ultimate-edition/plugins/terminal/fish/fish_variables
```







# Git

## Init

Run

```bash
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```

to set your account's default identity.

Omit `--global` to set the identity only in this repository.





## How to fix Git always asking for user credentials

- Make Git store the user name and password and it will never ask for them.

    ```bash
    git config --global credential.helper store
    ```







# Shell

## Fish

```bash
sudo pacman -S fish

set -U fish_greeting ""

fish_config
```



**Plugins**

> - [awesome.fish](https://github.com/jorgebucaran/awesome.fish)

- [fish-abbreviation-tips](https://github.com/Gazorby/fish-abbreviation-tips)

    ```bash
    fisher install Gazorby/fish-abbreviation-tips
    ```

- [z](https://github.com/jethrokuan/z) 

  **z** is a port of [z](https://github.com/rupa/z) for the [fish shell](https://fishshell.com/).

  ```bash
  fisher install jethrokuan/z
  ```

  



## ZSH

```bash
sudo pacman -S zsh-completions
```



trash-cli



- [Oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh)

    ```bash
    sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
    ```

- [Syntax highlight](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md)

    ```bash
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
    ```

- Autojump

    ```bash
    sudo pacman -S autojump
    ```

    Edit `~/.zshrc`:

    ```bash
    source /etc/profile.d/autojump.sh
    ```

- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md)

    ```bash
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
    ```

    







# Python

## Ipython and Jupyter Notebook

```bash
sudo pacman -S ipython jupyter-notebook
```









# Web

## VS Code

```bash
sudo pacman -S code
sudo pacman -S nodejs npm 
```





## Ruby

https://open.appacademy.io/learn/full-stack-online/software-engineering-foundations/ruby-environment-setup



```bash
sudo pacman -S ruby rubydocs
yay -S rbenv ruby-build
```

Edit `~/.zshrc`:

```bash
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"
```



### rbenv

```bash
# install Ruby version 2.5.1
rbenv install 2.5.1

# set version 2.5.1 to be our global default
rbenv global 2.5.1

# the 'rehash' command updates the environment to your configuration
rbenv rehash

# and let's verify everything is correct
# check the version
ruby -v # => 2.5.1

# check that we are using rbenv (this tells you where the version of ruby you are using is installed)
which ruby # => /Users/your-username/.rbenv/shims/ruby
```



### gem

```bash
gem install bundler pry byebug
```

