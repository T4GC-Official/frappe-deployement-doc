# Frappe Framework Version 15.x Installation Guide

This guide provides step-by-step instructions to install the Frappe Framework on your system.

## Prerequisites

Before you begin, ensure you have the following installed on your system:

- Python 3.6+ python3-dev python3-pip
- Node.js 20.x
- Redis 6.x
- MariaDB 10.6.6+
- yarn
- git
- wkhtmltopdf (with specific version requirements)
## Upadte the OS
Note: Before installing any software, it is recommended to update the OS to the latest version.
```sh
    sudo apt-get update
```

##  Install Git
Description: Git is a free and open-source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.
```sh
    sudo apt-get install git
```
## Install Redis
Description: Redis is an open-source, in-memory data structure store, used as a database, cache, and message broker.

```sh
    sudo apt-get install redis-server
```
## Install Python 3.6+
Description: Python is a programming language that lets you work quickly and integrate systems more effectively.

```sh
    sudo apt-get install python3 python3-dev python3-pip python3.10-venv -y
```
## Install MariaDB
Description: MariaDB is a community-developed, commercially supported fork of the MySQL relational database management system.
```sh
    sudo apt-get update
    sudo apt-get install mariadb-server
    sudo apt install software-properties-common
    mysql_secure_installation
    
```
Note: After running the above command, you will be prompted to set a root password, remove anonymous users, disallow root login remotely, remove the test database, and reload privileges. You can press Y and hit Enter for all the prompts.
#### Edit Configuration File if frappe version is less than v15.21.x 
#### Note: If you are using frappe version 15.21.x or above, you can skip this step.
```sh
    sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```
```vim
[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci

[mysql]
default-character-set = utf8mb4
```
#### Restart MariaDB
```sh
    sudo systemctl restart mariadb
```

## Install Node.js 20.x
Description: Node.js is an open-source, cross-platform, back-end JavaScript runtime environment that runs on the V8 engine and executes JavaScript code outside a web browser.
#### Install node using nvm (Node Version Manager)
```sh
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
    source ~/.profile 
    nvm install 20
    nvm use 20

```
#### Check Node version 
Note: The version should be 21.x or above. for latest version of frappe framework 15.x
```sh
    node -v
```

## Install Yarn using npm
Description: Yarn is a package manager that doubles down as project manager. Whether you work on one-shot projects or large monorepos, as a hobbyist or an enterprise user, we've got you covered.

```sh
    npm install -g yarn
```

#### Install **xvfb** is an X server that can run on machines with no display hardware and no physical input devices. It emulates a dumb framebuffer using virtual memory.
```sh
    sudo apt-get install -y xvfb 
```

#### Install **libfontconfig** is a library designed to provide system-wide font configuration, customization, and application access.
```sh
    sudo apt-get install -y libfontconfig
```

## Install wkhtmltopdf
**Description**: **wkhtmltopdf** and wkhtmltoimage are open source (LGPLv3) command line tools to render HTML into PDF and various image formats using the Qt WebKit rendering engine.
Download and install wkhtmltopdf package from https://wkhtmltopdf.org/downloads.html

```sh
  sudo apt-get install -y wkhtmltopdf
```

## Install Frappe Framework
Description: Frappe is a full-stack web application framework written in Python, JavaScript, HTML/CSS with MySQL as the backend. It was developed by Frappe Technologies Pvt. Ltd. and is released under the MIT license.

```sh
    pip3 install frappe-bench
```
 
Congratulations! You have successfully installed the Frappe Framework on your system.
<hr>
##################### Now the bench is ready to create new sites for development #####################
<hr>

# Production Setup

#### Install Nginx
```sh
    sudo apt-get install nginx
```

#### Install Supervisor
```sh
    sudo apt-get install supervisor
```

#### Install Fail2ban
```sh
    sudo apt-get install fail2ban
```
#### Install Certbot
```sh
    sudo apt-get install certbot python3-certbot-nginx -y
```
#### Create a new site
```sh
    bench new-site <site-name> --admin-password <site-admin-password> --db-root-password <mariadb-root-password>
```
#### Enable SSL for your site using Let's Encrypt but before that make sure your domain is pointing to the server's IP address.
you will also need to enable dns based multitenancy in site_config.json file
```sh
    sudo nano /home/frappe/frappe-bench/sites/<site-name>/site_config.json
```
```json
{
    "db_name": "site-name",
    "db_password": "site-db-password",
    "dns_multitenant": 1
}
```
or run the following command to enable dns based multitenancy
```sh
    bench config dns_multitenant on
```

#### Setup Redis Cache,Redis Queue and socketio
```sh
    bench setup redis
    bench setup socketio
```
#### Setup fail2ban
```sh
    sudo bench setup fail2ban
```
#### Setup Nginx
```sh
    sudo bench setup nginx
```

#### Setup Let's Encrypt
```sh
    sudo -H bench setup lets-encrypt <site-name>
```

#### Setup Supervisor
```sh
    sudo bench setup supervisor
```
#### Start Supervisor
```sh
    sudo supervisorctl relaod
    sudo supervisorctl status all
    sudo supervisorctl start all
```
# Localhost Development Setup
#### To enable developer mode
```sh
    bench set-config developer_mode 1
```
#### To enable auto-reload
```sh
    bench watch
```
#### To start the development server
```sh
    sudo supervisorctl restart all
```
# Install Frappe Apps
#### To install an app from the Frappe App Store
```sh
    bench get-app <app-name>
    bench --site <site-name> install-app <app-name>
```
#### To install an app from a custom repository
```sh
    bench get-app <app-repo-url>
    bench --site <site-name> install-app <app-name>
```
#### To uninstall an app
```sh
    bench --site <site-name> uninstall-app <app-name>
```

