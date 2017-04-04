# Requirements

Apache 2.4, PHP 7.0, MySQL 5.6, Node.js, npm

# Standards
For development code standards see https://github.com/DontPanicDigital/standards.

# Bin
For faster development, you can use the following binaries https://github.com/DontPanicDigital/bin.

# Git

Run this command

`git config branch.autosetuprebase always`

It will setup your local repository to use rebase instead of merge on `git pull`. Rebase is better because merge creates ugly and confusing patterns. Merge should be used only between different branches, not within a single branch.

Also, if you use PHPStorm git functionality (which you should!), set it up to use rebase on pull too.
 
![Screenshot of PHPStorm correct git settings](http://i.imgur.com/bxr40vL.png "Screenshot of PHPStorm correct git settings")

# Set up config file

Copy `/.htaccess.dist` to a new file `/.htaccess` and edit it.

Copy `/app/config/parameters.neon.dist` to a new file `/app/config/parameters.neon` and edit it.

Copy `/react/src/confg.js.dist` to a new file `/react/src/confg.js` and edit it:

* Set host to match your localhost setup.

Copy `/.deploy/config.json.dist` to a new file `/.deploy/config.json` and edit it:

* Only in case when you want to use a local deploy

# Redis

Redis is enabled by default for Journal, Storage and Sessions. Just setup Redis server at your device and install PHP Redis extension `brew install homebrew/php/php70-redis`.

On production environment, DB `0` is used for Journal and Storage. DB `1` for sessions.

For localhost, use whatever you want. There's `https://github.com/DontPanicDigital/bin` script you can use to quickly flush your local Redis from PhpStorm.

If you do not use Redis on a local machine, you can turn it off in section Redis `/app/config/parameters.neon`.

# Migrations

Whenever you need to change something in the database (for example you create or change some entity), you have to define new migrations file.

To learn more about `doctrine/migrations`, see http://docs.doctrine-project.org/projects/doctrine-migrations/en/latest/index.html.

#### Define Migrations

To generate new migrations file, run `php www/index.php migrations:generate`.
After generating new file, edit it to run desired SQL code.
Don't forget to implement both `up()` and `down()` methods to ensure that the new changes can be reverted.

The most common reason to create new migrations is, when you create or edit some entity.
Run `php www/index.php orm:schema-tool:update --dump-sql` to get SQL code. Then copy it to the new migrations file.
Then implement `down()` method to revert schema back to original state.

#### Run Migrations

To run new migrations, run `php www/index.php migrations:migrate`. This command is also called automatically when running `composer install`.

# Git flow

In our projects we use enhanced git system name git flow. Every project should be a git flow repo (you can turn classic repo into git-flow enabled one via command `git flow init`)
For more informations about git flow, installation process and usage visit `https://danielkummer.github.io/git-flow-cheatsheet/`
Our branches names are separated by `-` (example: `feature-new-feature` or `realease-v1.3.0`). There is no "version prefix" (one of `git init` questions), but we add it manually.
