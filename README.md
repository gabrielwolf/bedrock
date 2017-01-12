# [Bedrock + grunt-wordpress-deploy](https://github.com/gabrielwolf/bedrock-grunt-wordpress-deploy)

This fork of Bedrock and grunt-wordpress-deploy is made to deploy your site easily from and to different staging environments. It is useful when customers add content and you just have to pull/push the files and database to a staging or production environment.

## HowTo setup the baby on a local dev server and a staging server

Get Bedrock:  
`$ git clone https://github.com/gabrielwolf/bedrock-grunt-wordpress-deploy example.com && cd example.com`  

Get a good Grunt version for grunt-wordpress-deploy:  
`$ npm install`  

Get the latest grunt-wordpress-deploy:  
`$ git clone https://github.com/gabrielwolf/grunt-wordpress-deploy vendor/grunt-wordpress-deploy`  

Say NPM where to include grunt-wordpress-deploy:  
`$ npm link vendor/grunt-wordpress-deploy`  

Get WordPress, plugins, a theme and all the Bedrock stuff:  
`$ composer install && composer require wpackagist-theme/twentyfifteen`  

- Set up a local server (choose NGINX, Apache or what you prefer) name it example.dev and point the server root to `/var/www/example.com/web`  
- Create a local MYSQL database like `wp_example.com`  

Back to the web root make config files and adapt the credentials:  
`$ cp .env.example .env && nano .env`  
`$ cp Gruntfile.js.example Gruntfile.js && nano Gruntfile.js`  
  
- Open your browser go to example.dev and voila: WordPress should do fine by now.  
  
SSH to your staging server  
`$ ssh user@example.com`  
  
Prepare a directory for the website  
`$ cd /var/www/html`  
`$ mkdir example.com && cd example.com`  
  
Make a config file  
`touch .env`  
  
Adapt the credentials (you can copy & paste the content of your local .env to start with)  
`$ nano .env`  

- Set up your staging server, name it example.com with server root pointing to `/var/www/example.com/web`  
  
- Now we deploy the first time to our staging environment:  
`$ grunt push_files --target=staging`  
`$ grunt push_db --target=staging`  
  
- Open your browser and check example.com. A vanilla WordPress should say hello!

Troubleshooting:
If there are errors - and there will be, believe me - double check the configuration locally and on the server.
Mostly it's things like a missing trailing slash in the Gruntfile, or a typo in the domain name somewhere.
- Start on the files `.env` and `Gruntfile.js`.
- See if the servers point to the right directory.
- Run `composer install` locally.
- Match rsync versions on the developement and staging/live environment you're pushing to.
- Push files and database again. If there are errors while pushing, go to  `backups/` and read the \*.sql files, there you will find error messages.  
- Restart the servers.  
- If you have a white screen of death, probably the site url in the database on the server is wrong. Open example.com/wp/wp-admin, login, go to settings and see if the homeurl is http://example.com and the siteurl is http://example.com/wp.
- grunt-wordpress-deploy does automatically search and replace the urls before transfering the database up and down.

An error is mostly "just" a misconfiguration.  

Now the original Bedrock Readme:  

# [Bedrock](https://roots.io/bedrock/)
[![Packagist](https://img.shields.io/packagist/v/roots/bedrock.svg?style=flat-square)](https://packagist.org/packages/roots/bedrock)
[![Build Status](https://img.shields.io/travis/roots/bedrock.svg?style=flat-square)](https://travis-ci.org/roots/bedrock)

Bedrock is a modern WordPress stack that helps you get started with the best development tools and project structure.

Much of the philosophy behind Bedrock is inspired by the [Twelve-Factor App](http://12factor.net/) methodology including the [WordPress specific version](https://roots.io/twelve-factor-wordpress/).

## Features

* Better folder structure
* Dependency management with [Composer](http://getcomposer.org)
* Easy WordPress configuration with environment specific files
* Environment variables with [Dotenv](https://github.com/vlucas/phpdotenv)
* Autoloader for mu-plugins (use regular plugins as mu-plugins)
* Enhanced security (separated web root and secure passwords with [wp-password-bcrypt](https://github.com/roots/wp-password-bcrypt))

Use [Trellis](https://github.com/roots/trellis) for additional features:

* Easy development environments with [Vagrant](http://www.vagrantup.com/)
* Easy server provisioning with [Ansible](http://www.ansible.com/) (Ubuntu 16.04, PHP 7.1, MariaDB)
* One-command deploys

See a complete working example in the [roots-example-project.com repo](https://github.com/roots/roots-example-project.com).

## Requirements

* PHP >= 5.6
* Composer - [Install](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx)

## Installation

1. Create a new project in a new folder for your project:

  `composer create-project roots/bedrock your-project-folder-name`

2. Copy `.env.example` to `.env` and update environment variables:
  * `DB_NAME` - Database name
  * `DB_USER` - Database user
  * `DB_PASSWORD` - Database password
  * `DB_HOST` - Database host
  * `WP_ENV` - Set to environment (`development`, `staging`, `production`)
  * `WP_HOME` - Full URL to WordPress home (http://example.com)
  * `WP_SITEURL` - Full URL to WordPress including subdirectory (http://example.com/wp)
  * `AUTH_KEY`, `SECURE_AUTH_KEY`, `LOGGED_IN_KEY`, `NONCE_KEY`, `AUTH_SALT`, `SECURE_AUTH_SALT`, `LOGGED_IN_SALT`, `NONCE_SALT`

  If you want to automatically generate the security keys (assuming you have wp-cli installed locally) you can use the very handy [wp-cli-dotenv-command][wp-cli-dotenv]:

      wp package install aaemnnosttv/wp-cli-dotenv-command

      wp dotenv salts regenerate

  Or, you can cut and paste from the [Roots WordPress Salt Generator][roots-wp-salt].

3. Add theme(s) in `web/app/themes` as you would for a normal WordPress site.

4. Set your site vhost document root to `/path/to/site/web/` (`/path/to/site/current/web/` if using deploys)

5. Access WP admin at `http://example.com/wp/wp-admin`

## Deploys

There are two methods to deploy Bedrock sites out of the box:

* [Trellis](https://github.com/roots/trellis)
* [bedrock-capistrano](https://github.com/roots/bedrock-capistrano)

Any other deployment method can be used as well with one requirement:

`composer install` must be run as part of the deploy process.

## Documentation

Bedrock documentation is available at [https://roots.io/bedrock/docs/](https://roots.io/bedrock/docs/).

## Contributing

Contributions are welcome from everyone. We have [contributing guidelines](https://github.com/roots/guidelines/blob/master/CONTRIBUTING.md) to help you get started.

## Community

Keep track of development and community news.

* Participate on the [Roots Discourse](https://discourse.roots.io/)
* Follow [@rootswp on Twitter](https://twitter.com/rootswp)
* Read and subscribe to the [Roots Blog](https://roots.io/blog/)
* Subscribe to the [Roots Newsletter](https://roots.io/subscribe/)
* Listen to the [Roots Radio podcast](https://roots.io/podcast/)

[roots-wp-salt]:https://roots.io/salts.html
[wp-cli-dotenv]:https://github.com/aaemnnosttv/wp-cli-dotenv-command
