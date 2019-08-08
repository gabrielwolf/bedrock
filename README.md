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
- If you have a white screen of death, probably the site url in the database on the server is wrong. Open example.com/wp-admin, login, go to settings and see if the wordpress-address is http://example.com/wp and the site-address is http://example.com. Then update the permalinks.
- Start on the files `.env` and `Gruntfile.js`.
- See if both web servers point to the correct directories.
- Run `composer install` locally.
- Match rsync versions on the developement and staging/live environment you're pushing to.
- Push files and database again. If there are errors while pushing, go to  `backups/` and read the \*.sql files, there you will find error messages. 
- Restart the servers.
- grunt-wordpress-deploy does automatically search and replace the urls before transfering the database up and down.

An error is mostly "just" a misconfiguration.  

Now the original Bedrock Readme:  

# [Bedrock](https://roots.io/bedrock/)

[![Packagist](https://img.shields.io/packagist/v/roots/bedrock.svg?style=flat-square)](https://packagist.org/packages/roots/bedrock)
[![Build Status](https://img.shields.io/circleci/build/gh/roots/bedrock?style=flat-square)](https://circleci.com/gh/roots/bedrock)
[![Follow Roots](https://img.shields.io/twitter/follow/rootswp.svg?style=flat-square&color=1da1f2)](https://twitter.com/rootswp)

Bedrock is a modern WordPress stack that helps you get started with the best development tools and project structure.

Much of the philosophy behind Bedrock is inspired by the [Twelve-Factor App](http://12factor.net/) methodology including the [WordPress specific version](https://roots.io/twelve-factor-wordpress/).

## Features

* Better folder structure
* Dependency management with [Composer](https://getcomposer.org)
* Easy WordPress configuration with environment specific files
* Environment variables with [Dotenv](https://github.com/vlucas/phpdotenv)
* Autoloader for mu-plugins (use regular plugins as mu-plugins)
* Enhanced security (separated web root and secure passwords with [wp-password-bcrypt](https://github.com/roots/wp-password-bcrypt))

## Requirements

* PHP >= 7.1
* Composer - [Install](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx)

## Installation

1. Create a new project:
    ```sh
    $ composer create-project roots/bedrock
    ```
2. Update environment variables in the `.env` file:
  * Database variables
    * `DB_NAME` - Database name
    * `DB_USER` - Database user
    * `DB_PASSWORD` - Database password
    * `DB_HOST` - Database host
    * Optionally, you can define `DATABASE_URL` for using a DSN instead of using the variables above (e.g. `mysql://user:password@127.0.0.1:3306/db_name`)
  * `WP_ENV` - Set to environment (`development`, `staging`, `production`)
  * `WP_HOME` - Full URL to WordPress home (https://example.com)
  * `WP_SITEURL` - Full URL to WordPress including subdirectory (https://example.com/wp)
  * `AUTH_KEY`, `SECURE_AUTH_KEY`, `LOGGED_IN_KEY`, `NONCE_KEY`, `AUTH_SALT`, `SECURE_AUTH_SALT`, `LOGGED_IN_SALT`, `NONCE_SALT`
    * Generate with [wp-cli-dotenv-command](https://github.com/aaemnnosttv/wp-cli-dotenv-command)
    * Generate with [our WordPress salts generator](https://roots.io/salts.html)
3. Add theme(s) in `web/app/themes/` as you would for a normal WordPress site
4. Set the document root on your webserver to Bedrock's `web` folder: `/path/to/site/web/`
5. Access WordPress admin at `https://example.com/wp/wp-admin/`

## Documentation

Bedrock documentation is available at [https://roots.io/bedrock/docs/](https://roots.io/bedrock/docs/).

## Contributing

Contributions are welcome from everyone. We have [contributing guidelines](https://github.com/roots/guidelines/blob/master/CONTRIBUTING.md) to help you get started.

## Bedrock sponsors

Help support our open-source development efforts by [becoming a patron](https://www.patreon.com/rootsdev).

<a href="https://kinsta.com/?kaid=OFDHAJIXUDIV"><img src="https://cdn.roots.io/app/uploads/kinsta.svg" alt="Kinsta" width="200" height="150"></a> <a href="https://k-m.com/"><img src="https://cdn.roots.io/app/uploads/km-digital.svg" alt="KM Digital" width="200" height="150"></a>

## Community

Keep track of development and community news.

* Participate on the [Roots Discourse](https://discourse.roots.io/)
* Follow [@rootswp on Twitter](https://twitter.com/rootswp)
* Read and subscribe to the [Roots Blog](https://roots.io/blog/)
* Subscribe to the [Roots Newsletter](https://roots.io/subscribe/)
* Listen to the [Roots Radio podcast](https://roots.io/podcast/)
