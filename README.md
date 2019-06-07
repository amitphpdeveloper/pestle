[![Build Status](https://travis-ci.org/astorm/pestle.svg?branch=master)](https://travis-ci.org/astorm/pestle)

What is Pestle?
--------------------------------------------------
Pestle is

- A PHP Framework for creating and organizing command line programs
- An experiment in implementing python style module imports in PHP
- A collection of command line programs, with a primary focus on Magento 2 code generation

Pestle grew out of my desire to do something about the growing number of simple PHP scripts in my `~/bin` that didn't have a real home, and my personal frustration with the direction of PHP's namespace system.

PHP doesn't **need** another command line framework.  Symfony's console does a fine job of being the de-facto framework for building modern PHP command line applications.  Sometimes though, when you start off building something no one needed, you end up with something nobody realized they wanted.

How to Use
--------------------------------------------------
The easiest way to get started is to grab the latest build using curl

    curl -LO http://pestle.pulsestorm.net/pestle.phar

You can see a list of commands with the following

    php pestle.phar list-commands

and get help for a specific command with

    php pestle.phar help generate_module

If you want to build your own `phar`, we've got a `phing` `build.xml` file setup so all you *should* need to do to build a stand alone `pestle.phar` executable is

- `git checkout git@github.com:astorm/pestle.git`
- composer.phar install
- ./build.sh (which, in turn, calls the `phing` job that builds the `phar`

If you're interested in working on the framework itself, you can use the `runner.php` in the project root.  I personally use it by dropping the following in my `~/bin`.

    #File: ~/bin/pestle_dev
    #!/usr/bin/env php
    <?php
    require_once('/Users/alanstorm/Documents/github/astorm/pestle/runner.php');

Troubleshooting Upgrades
--------------------------------------------------
If you've upgraded pestle and you're seeing the following exception

> PHP Fatal error:  Cannot redeclare Pulsestorm\Magento2\Cli\Help\pestle_cli()

Try removing the following temp folder.

    /tmp/pestle_cache

We know this isn't ideal, and we're working on a more permanat fix.


Example Command
--------------------------------------------------

Try

    $ pestle.phar generate_module

from a Magento 2 sub-directory to get an idea of what we're doing here.

How to Use Pestle Code in your Application
--------------------------------------------------
Pestle and the `pestle_import` function are a bit of an experiment, and you probably don't want to run code from `module.php` files directly in your PHP based application.  Fortunately, we have a solution for you -- with every release of pestle we build a composer compatible autoloader in `library/autoloader.php`. This loads the entire pestle library structure as a single PHP file with proper block-namespaces (currently `library/all.php`).  This means you can include pestle in your Composer based projects with

    "require": {
        "pulsestorm/pestle": "1.0.*"
    }

And then import pestle code via native PHP namespaces to your heart's content.

    //include is probably not neccesary, usually handled by your framework
    include 'vendor/autoload.php';
    \Pulsestorm\Pestle\Library\output("Hello World");

Our specific strategy around this may change in the future, but our plan is for these sorts of changes to be user-transparent.  If we ever split the generated library into multiple files, or figure out a sane way to incorporate `pestle_import` into native PHP code and you're using this project as a composer library — those changes should be transparent to you.

Do you have strong options about this sort of compilation/"transpiling"/module-importing?  We'd love to have you involved in the project. Yell at us in a GitHub issues and/or pull request.

Want to learn more?  We'll [be using the wiki](https://github.com/astorm/pestle/wiki) for documentation until we outgrow it.

Experimental Tab Completion
--------------------------------------------------
Pestle includes an experimental [tab completion script](https://github.com/astorm/pestle/blob/master/pestle-autocomplete.sh).  If used with your system's `bash_completion` sub-system, this script will allow use the `[tab]` key to auto-complete command names.

    $ pestle.phar magento2:generate:ui: (press the tab key)
    add-column-text    add-schema-column  form
    add-form-field     add-to-layout      grid

Just copy or symlink the [`pestle-autocomplete.sh`](https://github.com/astorm/pestle/blob/master/pestle-autocomplete.sh) file to your `bash_completion.d` folder and you'll be good to go.

If you're running MacOS or MacOS X, you'll need to install the modern version of `bash_completion` via Homebrew (or your package manager of choice).  Yes, this is super annoying.  We [found these instructions](https://www.simplified.guide/macos/bash-completion) useful in late mid-2018.  The simplified instructions are

1. Install [Homebrew](https://brew.sh/)
2. Run `$ brew install bash-completion` to install the bash-completion package
3. Enable the completion scripts by running `$ . /usr/local/etc/bash_completion` -- optionally adding this command (or a similar one) to your `.bash_profile`

Available commands:
--------------------------------------------------
Alanstormdotcom
  alanstormdotcom:rsync                      One Line Description

Codecept
  codecept:convert-selenium-id-for-codecept  Converts a selenium IDE html test for conception

Faker
  faker:names                                Creates some Fake Name

Magento1 Convert
  magento1:convert:generate-maps             ALPHA: Wrapper for Magento's code-migration tools
  magento1:convert:magentoinc                ALPHA: Wrapper for Magento Inc.'s code-migration tool
  magento1:convert:unirgy                    ALPHA: Wrapper for Unirgy Magento Module Conversion

Magento2
  magento2:base-dir                          Output the base magento2 directory
  magento2:check-templates                   Checks for incorrectly named template folder
  magento2:class-from-path                   Turns a Magento file path into a PHP class
  magento2:class-list                        Get a list of all of magento2's extensible classes
  magento2:convert-class                     ALPHA: Partially converts Magento 1 class to Magento 2
  magento2:convert-observers-xml             ALPHA: Partially converts Magento 1 config.xml to Magento 2
  magento2:convert-system-xml                ALPHA: Partially Converts Magento 1 system.xml into Magento 2 system.xml
  magento2:extract-mage2-system-xml-paths    Generates Mage2 config.xml
  magento2:fix-direct-om                     ALPHA: Fixes direct use of PHP Object Manager
  magento2:fix-permissions-modphp            ALPHA: "Fixes" permissions for development boxes
  magento2:path-from-class                   Turns a PHP class into a Magento 2 path
  magento2:read-rest-schema                  BETA: Magento command, reads the rest schema on a Magento system

Magento2 Code-migration
  magento2:code-migration:rename             ALPHA: Rename .converted files

Magento2 Generate
  magento2:generate:acl                      Generates a Magento 2 acl.xml file.
  magento2:generate:acl:change-title         Changes the title of a specific ACL rule in a Magento 2 acl.xml file
  magento2:generate:class-child              Generates a child class, pulling in __constructor for easier di
  magento2:generate:command                  Generates bin/magento command files
  magento2:generate:config-helper            Generates a help class for reading Magento's configuration
  magento2:generate:controller-edit-acl      Edits the const ADMIN_RESOURCE value of an admin controller
  magento2:generate:crud-model               Generates a Magento 2 CRUD/AbstractModel class and support files
  magento2:generate:di                       Injects a dependency into a class constructor
  magento2:generate:full-module              Creates shell script with all pestle commands needed for full module output
  magento2:generate:install                  BETA: Generates commands to install Magento via composer
  magento2:generate:menu                     Generates configuration for Magento Adminhtml menu.xml files
  magento2:generate:module                   Generates new module XML, adds to file system
  magento2:generate:observer                 Generates Magento 2 Observer
  magento2:generate:plugin-xml               Generates plugin XML
  magento2:generate:preference               Generates a Magento 2.1 ui grid listing and support classes.
  magento2:generate:psr-log-level            For conversion of Zend Log Level into PSR Log Level
  magento2:generate:registration             Generates registration.php
  magento2:generate:remove-named-node        Removes a named node from a generic XML configuration file
  magento2:generate:route                    Creates a Route XML
  magento2:generate:schema-upgrade           BETA: Generates a migration-based UpgradeSchema and UpgradeData classes
  magento2:generate:service-contract         ALPHA: Service Contract Generator
  magento2:generate:theme                    Generates Theme Configuration
  magento2:generate:ui:add-column-text       Adds a simple text column to a UI Component Grid
  magento2:generate:ui:add-form-field        Adds a Form Field
  magento2:generate:ui:add-form-fieldset     Add a Fieldset to a Form
  magento2:generate:ui:add-schema-column     Genreated a Magento 2 addColumn DDL definition and inserts into file
  magento2:generate:ui:add-to-layout         Adds a <uiComponent/> node to a named node in a layout update XML file
  magento2:generate:ui:form                  Generates a Magento 2 UI Component form configuration and PHP boilerplate
  magento2:generate:ui:grid                  Generates a Magento 2.1 ui grid listing and support classes.
  magento2:generate:view                     Generates view files (layout handle, phtml, Block, etc.)

Magento2 Scan
  magento2:scan:acl-used                     Scans modules for ACL rule ids, makes sure they're all used/defined
  magento2:scan:class-and-namespace          BETA: Scans a Magento 2 module for misnamed PHP classes
  magento2:scan:htaccess                     ALPHA: Checks for missing Magento 2 HTACCESS files from a hard coded list
  magento2:scan:registration                 Scans Magento 2 directories for missing registration.php files

Magento2 Search
  magento2:search:search-controllers         Searches controllers

Mysql
  mysql:key-check                            Looks for Invalid Keys in a MySQL Database

Nexmo
  nexmo:send-text                            Sends a text message
  nexmo:store-credentials                    Stores Nexmo API in temp file
  nexmo:verify-request                       Sends initial request to verify user's phone number
  nexmo:verify-sendcode                      Nexmo Verify API: Second Step

Parsing
  parsing:citicard                           BETA: Parses Citicard's CSV files into yaml
  parsing:csv-to-iif                         BETA: Converts a CSV file to .iif

Pestle
  pestle:baz-bar                             Another Hello World we can probably discard
  pestle:build-command-list                  Builds the command list
  pestle:clear-cache                         BETA: Clears the pestle cache
  pestle:dev-import                          Another Hello World we can probably discard
  pestle:dev-namespace                       BETA: Used to move old pestle files to module.php -- still needed?
  pestle:export-as-symfony                   Exports a Pestle Module as a Symfony Console Command
  pestle:export-module                       ALPHA: Seems to be a start at exporting a pestle module as functions.
  pestle:foo-bar                             ALPHA: Another Hello World we can probably discard
  pestle:generate-command                    Generates pestle command boiler plate
  pestle:hello-argument                      A demo of pestle argument and option configuration/parsing
  pestle:pestle-run-file                     ALPHA: Stub for running a single PHP file in a pestle context

Php
  php:extract-session                        ALPHA: Extracts data from a saved PHP session file
  php:format-php                             ALPHA: Experiments with a PHP formatter.
  php:test-namespace-integrity               ALPHA: Tests the "namespace integrity?  Not sure what this is anymore.

Postscript
  postscript:check                           Outputs the PostScript code needed to print a check

Pulsestorm
  pulsestorm:build-book                      BETA: Command for building No Frills Magento 2 Layout
  pulsestorm:md-to-say                       Converts a markdown files to an aiff
  pulsestorm:monty-hall-problem              Runs Simulation of "Monty Hall Problem"
  pulsestorm:orphan-content                  BETA: Used to scan my old pre-Wordpress archives for missing pages.
  pulsestorm:pandoc-md                       BETA: Uses pandoc to converts a markdown file to pdf, epub, epub3, html, txt
  pulsestorm:solo-noble                      One Line Description

Uncategorized
  hello-world                                A Hello World command.  Hello World!
  help                                       Alias for list
  list-commands                              Lists help
  selfupdate                                 Updates the pestle.phar file to the latest version
  test-output                                A test command for the output function that should probably be pruned
  testbed                                    Test Command
  version
