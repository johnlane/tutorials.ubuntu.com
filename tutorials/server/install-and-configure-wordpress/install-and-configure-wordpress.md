---
id: install-and-configure-wordpress
summary: Install and configure WordPress, an online publishing platform, and create your first post.
categories: server
tags: ubuntu-server, apache, wordpress, blogging, cms
difficulty: 3
status: published
feedback_url: https://github.com/canonical-websites/tutorials.ubuntu.com/issues
published: 2018-06-19
author: Marcin Mikołajczak <me@m4sk.in>

---

# Install and configure WordPress

## Overview
Duration: 1:00

WordPress is open source software that you can use to create a beautiful website, blog, or app. It is the most popular online publishing platform, currently powering more than 28% of the web. 

It is based on PHP and MySQL, and runs on the Apache2 web server. Its features can be extended with thousands of free plugins and themes. 

In this tutorial we will install WordPress and create our first blog post.

### What you’ll learn

  - How to set up WordPress
  - How to configure WordPress
  - How to create your first post

### What you’ll need

  - Ubuntu 18.04 LTS (Server or Desktop)

Your server's ready? Let's get started...

## Install WordPress
Duration: 1:00

Run these commands to install WordPress and its dependencies Apache2, PHP and MySQL:

```bash
sudo apt update
sudo apt install php apache2 mysql-server libapache2-mod-php php-mysql wordpress
```

Confirm that the installed services are running (look for `active (running)` status):
```bash
sudo systemctl status mysql
sudo systemctl status apache2
```

## Configure Apache for WordPress
Duration: 2:00

Use a text editor (with `sudo` for write access) to create a new Apache site configuration file for your WordPress site:
```sh
sudo nano /etc/apache2/sites-available/wordpress.conf
```
Enter the following site configuation then save and exit from your editor:
```
Alias /blog /usr/share/wordpress
<Directory /usr/share/wordpress>
    Options FollowSymLinks
    AllowOverride Limit Options FileInfo
    DirectoryIndex index.php
    Order allow,deny
    Allow from all
</Directory>
<Directory /usr/share/wordpress/wp-content>
    Options FollowSymLinks
    Order allow,deny
    Allow from all
</Directory>
```
Enable the site:
```sh
sudo a2ensite wordpress
sudo systemctl reload apache2
```

## Create a database
Duration: 4:00

WordPress needs a MySQL database. Start the MySQL shell so that we can set one up:

```sh
sudo mysql -u root
```
A welcome message will greet you and present a command prompt:
```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 7
Server version: 5.7.22-0ubuntu0.18.04.1 (Ubuntu)

mysql>
```

First create the database (_notice how MySQL commands end with a semicolon!_):
```
CREATE DATABASE wordpress;
```
MySQL will respond like this:
```
Query OK, 1 row affected (0,00 sec)
```
Next, grant access rights to the database:
```
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
ON wordpress.*
TO wordpress@localhost
IDENTIFIED BY '<your-password>';
FLUSH PRIVILEGES;
```

That's it. Enter `quit` to leave the MySQL shell. It'll exit, responding with:
```
Bye
```

In the next step, we'll introduce WordPress to its database...

## Configure Wordpress
Duration: 2:00

Before we can configure Wordpress we must tell it about its database. Use a text editor (with `sudo`) to create a new configuration file for the database connection parameters:
```sh
sudo nano /etc/wordpress/config-localhost.php
```
Write the following into the file, then save it and exit from your editor:
```
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', '<your-password>');
define('DB_HOST', 'localhost');
define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
?>
```

Now point your browser to `http://localhost/blog` to view the setup page where you should enter a title for your new site, your e-mail address (which is used for password resets), and choose a user name and password (a random password is provided for you but you are free to change it).

![WordPress installation](images/install.png)

A final option allows you to decide if your site should be indexed by search engines. 

Press the **Install WordPress** button at the bottom of the page when you have completed the form. You should see a page informing you that WordPress has been successfully installed.

You can now press the **Log In** button (or browse to `http://localhost/blog/wp-login.php`) to log into our new WordPress *Dashboard* where we can start writing our first post.

## Write your first post

The Dashboard is your home within WordPress. A panel welcomes you to WordPress and offers links to help you get started. 

![Dashboard](images/dashboard.png)

Start with the link inviting you to **Write your first blog post**. It brings up the **Add New Post** screen.

positive
: You can also start a new post using the **new** button on the top bar or by selecting **Posts > Add New** from the side bar.

![First post](images/new-post.png)

Enter title in the space provided and then write your content in the editing area below.

The editing area has two modes selectable by tabs along the top: **Visual** is the default and has simple (but powerful) text formatting options, whereas the **Text** mode allows you to write using HTML.

Go ahead... Write your post and, when it's ready,  publish it: the **Publish** button is over there on the right-hand side of the screen.

You can now view your brand new post - WordPress gives you a *Permalink* to it. It's right there, just beneath the title!

## You've done it!
Duration: 1:00

Congratulations! You have installed WordPress and started posting content.

Of course, this tutorial is only a basic introduction to WordPress. You can do much more with it, perhaps install one of thousands of available (free and commercial) plugins and themes. Or use it as forum (with [bbPress] plugin), microblogging platform ([BuddyPress]), eCommerce platform ([WooCommerce]) or extend existing WordPress features with plugins like [JetPack] or [TinyMCE Advanced][tinymce-advanced].

Read the WordPress documentation, available in the [WordPress Codex][wordpress-codex], to discover more about WordPress usage or even theme/plugin development.

Help is always at hand should you need more guidance on using WordPress:

* [Ask Ubuntu][askubuntu]
* [Ubuntu Forums][forums]
* [WordPress.org Forums][wpforums]

### Further reading:

* [WordPress Codex][wordpress-codex]
* [WordPress page in Ubuntu documentation][ubuntu-docs]

<!-- LINKS -->
[bbPress]: https://bbpress.org/
[BuddyPress]: https://buddypress.org/
[WooCommerce]: https://woocommerce.com/
[JetPack]: https://jetpack.com/
[tinymce-advanced]: http://www.laptoptips.ca/projects/tinymce-advanced/
[wordpress-codex]: https://codex.wordpress.org/
[askubuntu]: https://askubuntu.com/
[forums]: https://ubuntuforums.org/
[wpforums]: https://wordpress.org/support/forums/
[ubuntu-docs]: https://help.ubuntu.com/lts/serverguide/wordpress.html