This repo contains a Bedrock installation for Pressbooks. In includes pressbooks/pressbooks, several themes, and open source plugins. It is intended to be used on a production server.

## Requirements

- PHP >= 8.2
- [WP CLI](https://wp-cli.org/#installing)
- [Composer](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx)
- For PDF exports: PrinceXML or [Docraptor](https://docraptor.com/), a SaaS version of PrinceXML. Note: PrinceXML is not free software; see [their license agreement](https://www.princexml.com/license/). If you intend to use Prince for commercial purposes, you should [purchase a license](https://www.princexml.com/purchase/).
- For the Cover Generator feature, install:
    Ghostscript: `sudo apt-get install ghostscript`
    ImageMagick: `sudo apt-get install imagemagick`
    PdfToPpm and PdfInfo: `sudo apt-get install poppler-utils`
- To produce EPUB exports, install EPUBCheck, an EPUB validation utility used in the export process: `sudo apt-get install epubcheck`.
- To produce XML exports, install xmllint: `sudo apt-get install libxml2-utils`
- To include math expressions in your export files, install and host the [pb-mathjax](https://github.com/pressbooks/pb-mathjax/?tab=readme-ov-file#deploy-to-a-production-server) service [additional community recommended install instructions](https://pressbooks.community/t/improved-support-for-mathematical-scientific-notation/4156/4) or install and network activate the third party WordPress plugin WP QuickLaTeX.


## Installing Pressbooks on a Production Server

1. Install WP CLI, Composer, and all required server dependencies 
2. Setup the document root on your webserver to Bedrock's `web` folder: `/path/to/site/web/`
3. Checkout this branch and run `composer install`.
4. Install [WordPress multisite via CLI command](https://developer.wordpress.org/cli/commands/core/multisite-install/), replacing the example values provided here with your desired values: `wp core multisite-install --url="https://example.com" --title="Your Network Title" --admin_user="username" --admin_password="password" --admin_email="youremail@example.com"`
5. Copy the provided .env.example file to .env: `cp .env.example .env` 
6. Update the relevant environment variables in your new `.env` file. Wrap values that may contain non-alphanumeric characters with quotes, or they may be incorrectly parsed.

- Database variables
  - `DB_NAME` - Database name
  - `DB_USER` - Database user
  - `DB_PASSWORD` - Database password
  - `DB_HOST` - Database host
  - Optionally, you can define `DATABASE_URL` for using a DSN instead of using the variables above (e.g. `mysql://user:password@127.0.0.1:3306/db_name`)
- `WP_ENV` - Set to environment (`development`, `staging`, `production`)
- `WP_HOME` - Full URL to WordPress home (https://example.com)
- `WP_SITEURL` - Full URL to WordPress including subdirectory (https://example.com/wp)
- `AUTH_KEY`, `SECURE_AUTH_KEY`, `LOGGED_IN_KEY`, `NONCE_KEY`, `AUTH_SALT`, `SECURE_AUTH_SALT`, `LOGGED_IN_SALT`, `NONCE_SALT`
  - Generate with [wp-cli-dotenv-command](https://github.com/aaemnnosttv/wp-cli-dotenv-command)
  - Generate with [our WordPress salts generator](https://roots.io/salts.html)
- `DOCRAPTOR_API_KEY` - If you choose to use DocRaptor to generate PDFs, enter the API key obtained from them here.
- `PB_MATHJAX_URL` - [PB-MathJax](https://github.com/pressbooks/pb-mathjax) is a hostable microservice that can generate SVG or PNG images of math expressions for use in EPUB and PDF exports. If you self-host, provide the URL of your hosted service here.

7. Activate Pressbooks and any desired themes and plugins via WP CLI, e.g. `wp plugin activate pressbooks --network`
8. Log in to your new Pressbooks network at `https://example.com/wp/wp-login.php`

## About Bedrock
[Bedrock](https://roots.io/bedrock/) is a modern WordPress stack that helps you get started with the best development tools and project structure. Much of the philosophy behind Bedrock is inspired by the [Twelve-Factor App](http://12factor.net/) methodology including the [WordPress specific version](https://roots.io/twelve-factor-wordpress/). Bedrock includes:

- Better folder structure
- Dependency management with [Composer](https://getcomposer.org)
- Easy WordPress configuration with environment specific files
- Environment variables with [Dotenv](https://github.com/vlucas/phpdotenv)
- Autoloader for mu-plugins (use regular plugins as mu-plugins)
- Enhanced security (separated web root)

The core Bedrock project is maintained by [Roots](https://roots.io/about/), a small team of open source developers who have been publishing tools for WordPress developers since 2011. You can support their work by purchasing [Radicle](https://roots.io/radicle/), their recommended WordPress stack, or by [sponsoring them](https://github.com/sponsors/roots) on GitHub. 