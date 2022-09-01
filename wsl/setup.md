# WSL Setup for Web Development

I hate windows, so I'm trying to avoid using it as much as possible for my
actual development work. So I'm documenting how to set up a WSL environment for
web development.

This guide assumes WSL2.

## Install a distro

The obvious choice here is whatever the latest version of Ubuntu is available on
the Windows Marketplace or whatever it's called.

Once that's up and running, you'll need to set up a user and password and all
that goodness. I recommend keeping this the same as your Windows user, just for
consistency.

## Setup some SSH Keys:

First generate some keys:

```
ssh-keygen -t rsa -b 4096 -C "dean@harris.tc" -f $HOME/.ssh/id_rsa -P ""
ssh-keygen -t rsa -b 4096 -C "dean@repeat.gg" -f $HOME/.ssh/work_rsa -P ""
```

Then use them where I need to. Primarily git providers:

- [Github (Personal)](https://github.com/settings/ssh/new)
- [Bitbucket (Work)](https://bitbucket.org/account/settings/ssh-keys/)

## Update Apt Repos

To ensure that the full breadth of the packages on Apt are available, and up to
date, update the Apt repos on your machine:

```
sudo apt update
```

You may also want to ensure that there are no upgrades available, by running apt
upgrade:

```
sudo apt upgrade
```

## Install required packages

Depending on what flavour of web development you do, you'll likely need some
slightly different packages than I do, but at the very least some of them will
be the same. Here is my command:

```
sudo apt-get install apache2 php mongodb mysql-server fzf neovim composer php7.4-curl php7.4-dom php7.4-simplexml php7.4-xml
```

```shell
sudo apt-get install \
  php7.4 \
  php7.4-mbstring \
  php7.4-curl \
  php7.4-mysql \
  php7.4-xml \
  php7.4-dom
```

## Configure Apache

Enable apache to work outside of the predefined directories by creating a
`<Directory>` definition in `/etc/apache2/apache2.conf`, changing the path to
wherever you will clone repositories to

```
<Directory /home/deanacus/dev/>
  Options Indexes Includes FollowSymLinks MultiViews
  AllowOverride All
  Require all granted
</Directory>
```

## Configure MySQL

You won't be able to connect to mysql as root without `sudo`, thanks to the
default authentication setting of `auth_socket`. We can fix this, though, by
connecting via `sudo mysql -u root`, and reconfiguring the root user.

I suggest, firstly, that you create a user with the same username as your
current user, with no password, and grant that user super user privileges. Just
in case something goes wrong:

```
CREATE USER 'deanacus'@'localhost' IDENTIFIED WITH mysql_native_password BY '';
GRANT ALL PRIVILEGES ON *.* to 'deanacus'@'localhost';
FLUSH PRIVILEGES;
```

Then, change the authentication values for `root`:

```
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '';
```

Now, you should be able to run any mysql commands you need as either root or
yourself without needing a password.

## Install Node

The version of Node that exists in the Ubuntu repositories is super out of date.
So we'll need to add a new repository in order to install an up to date version.

```
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash && sudo apt-get install -y nodejs
```

## Yarn

If you like to use the Yarn package manager, you'll need to add another repo, as
the yarn package on the Ubuntu repos is unrelated. The steps are documented on
the Yarn website, but I'll just list them here for ease:

```
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add - && echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list && sudo apt update && sudo apt install --no-install-recommends yarn
```

## Set up Symfony Repo:

Clone the repository to wherever you keep your source code (get the repo from
bitbucket or confluence)

Navigate to `/etc/apache2/sites-available` and run
`sudo cp 000.default.conf 001.devlocal.conf`.

Edit the newly created file and insert the following configuration, replacing
the DocumentRoot and Directory values to wherever you have the symfony repo:

```
<VirtualHost *:80>
  ServerName devlocal.repeat.gg
  ServerAlias devlocal.repeat.gg
  ErrorLog /var/log/apache2/error.log
  CustomLog /var/log/apache2/access.log combined
  DocumentRoot /home/deanacus/dev/symfony/web
  <Directory /home/deanacus/dev/symfony/web>
    Order Deny,Allow
    Allow from all
  </Directory>
</VirtualHost>
```

Create a symlink for this file in `/etc/apache2/sites-enabled` by running

```
sudo ln -sf /etc/apache2/sites-available/001.devlocal.conf /etc/apache2/sites-enabled/001.devlocal.conf
```

Now follow the rest of the steps in Confluence to finish setting up the
application.

## Access the machine

WSL2 is purely a Hyper-V virtual machine, without bridged networks. That means
that every time WSL starts, its IP changes, thus requiring an update to the
windows hosts file. I have a little shell script setup to manage that:

```
#! /bin/bash
ip=$(ip addr list eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}')


# Update the Windows hostfile, because the networks aren't bridged
echo "127.0.0.1 localhost
${ip} devlocal.repeat.gg
::1 localhost" > /mnt/c/Windows/System32/drivers/etc/hosts
```

## Enable GUI apps to run.

In your shell configuration, export a variable called 'DISPLAY' that has the
value equvivalent to "$(cat /etc/resolv.conf | grep -Po '\d+(\.\d+){3}'):0.0",
then install VCXSRV to run an x11 server on the windows machine that they linux
box can connect to, to launch gui apps
