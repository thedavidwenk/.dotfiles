This guide will help you get your development environment set up with your dotfiles stored in a version controlled repository.

I got inspried by Anand Iyer's Blog Post: [A Simpler Way to Manage your Dotfiles](https://www.anand-iyer.com/blog/2018/a-simpler-way-to-manage-your-dotfiles/)

## First Time Setup

### Creating the Repository:

Make a directory: Create a hidden directory in your home directory named .dotfiles to store your configuration files.

    mkdir $HOME/.dotfiles

### Initialize the Git repository: 

We'll use git init to create a bare Git repository inside the .dotfiles directory. This means the actual Git data is stored separately from your working files.
Bash

    git init --bare $HOME/.dotfiles

### Setting Up an Alias:

Create an alias: An alias is a shortcut for a longer command. We'll create an alias named dotfiles to simplify running Git commands on your dotfiles repository. You can customize the alias name to your preference.
    
    alias dotfiles='/usr/local/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'

    Add the alias to your shell configuration: We need to add the alias definition to your shell configuration file (e.g., .bashrc or .zshrc). This will make the dotfiles alias available whenever you open your terminal.

### Configuring the Repository:

Hide untracked files: By default, Git shows untracked files (files not yet added to the repository). We can configure Git to ignore these for a cleaner status view.   

    dotfiles config --local status.showUntrackedFiles no

Add a remote repository: A remote repository is a copy of your Git repository hosted on GitHub. You'll need to replace git@github.com:thedavidwenk/dotfiles.git with the URL of your own remote repository.
Bash

    dotfiles remote add origin git@github.com:your-username/your-dotfiles-repo.git

### Adding Configuration Files:

Now you can start adding your configuration files (e.g., .tmux.conf) to version control.

Navigate to your home directory:

    cd $HOME

    dotfiles add gitconfig
    dotfiles commit -m "Add gitconfig"
    dotfiles push -u origin master

# Setting Up a New Machine

To use your dotfiles on a new machine, follow these steps:

Clone the repository: Use git clone with the --separate-git-dir flag to specify the location of the bare repository and your home directory. Replace https://github.com/thedavidwenk/dotfiles.git with the URL of your remote repository.

    git clone --separate-git-dir=$HOME/.dotfiles https://github.com/your-username/your-dotfiles-repo.git ~


If your home directory already contains configuration files, cloning might fail because of conflicts. As a workaround, you can clone the repository into a temporary directory, copy the files to your home directory, and then remove the temporary directory.


```
    git clone --separate-git-dir=$HOME/.dotfiles https://github.com/thedavidwenk/dotfiles.git tmpdotfiles
    rsync --recursive --verbose --exclude '.git' tmpdotfiles/ $HOME/
    rm -r tmpdotfiles

```

Source / Inspiration: https://www.anand-iyer.com/blog/2018/a-simpler-way-to-manage-your-dotfiles/
