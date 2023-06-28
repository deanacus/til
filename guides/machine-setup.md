New Machine Setup Quickstart

This is generally dependent somewhat on what platform we're working on but here's the basic lowdown:

* Get Identity Stuff Sorted
	* Generate some SSH Keys
	* Set user password

* Install Programming Tools
	* Git
	* Node/NPM
	* Yarn
	* Apache
	* PHP
	* Composer
	* MySQL
	* MongoDB

* Install CLI Tools
	* Fish
	* Neovim
	* Tmux
	* FZF
	* FD
	* Ripgrep
	* Bat
	* Exa

* Install some GUI Tools
	* Visual Studio Code
	* Firefox (Developer Edition)
	* Google Chrome
	* Slack
	* MySQLWorkbench
	* 1Password
	* Figma


---

Install Homebrew

`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

Install packages

`brew install bat composer curl exa fd fish fnm fzf git httpd neovim php@7.4 ripgrep tmux wget`

Install apps

`brew install --cask 1password discord docker figma firefox-developer-edition google-chrome hyper mysqlworkbench postman rectangle runjs slack spotify studio-3t visual-studio-code`

Clone Dotfiles

`git clone https://github.com/deanacus/dotfiles ~/dotfiles`