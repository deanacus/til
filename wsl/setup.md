# WSL Setup for Web Development

I hate windows, so I'm trying to avoid using it as much as possible for my
actual development work. So I'm documenting how to set up a WSL environment
for web development.

This guide assumes WSL2.

## Install a distro

The obvious choice here is whatever the latest version of Ubuntu is available
on the Windows Marketplace or whatever it's called.

Once that's up and running, you'll need to set up a user and password and
all that goodness. I recommend keeping this the same as your Windows user,
just for consistency.

## Install required packages

Depending on what flavour of web development you do, you'll likely need some
slightly different packages than I do, but at the very least some of them
will be the same. Here is my command:

```
sudo apt-get install apache2 php7.4 mongodb mariadb-server yarn fzf ripgrep neovim
```

## Configure Apache

Very little to do here. Mostly just needed to add a site to the `sites-available`
folder, and create a symlink into the `sites-enabled` folder.

## Configure PHP

In order to configure PHP to run as expected locally, you will need to install
some additional packages and enable them in the php.ini file. 

Install the packages:

```
sudo apt-get install php7.4-bcmath php7.4-curl php7.4-mbstring php7.4-mysql php7.4-sqlite3 php7.4-xml php7.4-zip
```

Enable the extensions you will need. Mine for work look like this:

```
extension=bz2
extension=curl
extension=fileinfo
extension=gd2
extension=gettext
extension=gmp
extension=intl
extension=imap
extension=ldap
extension=mbstring
extension=exif
extension=mysqli
extension=openssl
extension=pdo_mysql
extension=pdo_sqlite
extension=soap
extension=sockets
extension=sqlite3
extension=xmlrpc
extension=xsl
```

## Configure MySQL/MariaDB

Once MySQL is up and running, you will struggle to connect using a password,
due to the fact that it is configured to require a socket based connection by
default. You can use `sudo` to connect with a password, but this isn't feasible
for development tooling, as it won't have sudo access.

If mysql won't start because of a missing `mysql.sock` file, check the server config
file at `/etc/mysql/mariadb.conf.d/50-server.cnf`, and if necessary, change the
bind-address setting from `127.0.0.1` to `localhost`. This *should* allow it to run.

Connect to the mysql instance by running `sudo mysql -u root` and entering your
Linux (not mysql) password. Once connected, verify that the root user exists by
running

```
SELECT * FROM mysql.user WHERE user='root' \G
```

You should see the details for the root user, including Host as 'localhost', 
and plugin as 'unix_socket'. To enable password based connections for the root
user, run

```
UPDATE mysql.user SET plugin = '', Host = '%' WHERE user = 'root';
```

## Install Node

The version of Node that exists in the Ubuntu repositories is super out of date. So we'll need to add a new repository in order to install an up to date version.

```
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
```

Now we can run

```
sudo apt-get install -y nodejs
```

## Yarn

If you like to use the Yarn package manager, you'll need to add another repo, as the yarn package on the Ubuntu repos is unrelated. The steps are documented on the
Yarn website, but I'll just list them here for ease:

```
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update
sudo apt install --no-install-recommends yarn
```

## Access the machine

Because WSL2 is just a virtual machine with little to no configuration in terms
of how it is virtualised, the network is no longer an extension of the host
network, but a separate network entirely. One that changes every time something
to do with the VM's network changes (usually a system restart of either the 
Linux VM, or the host machine).
