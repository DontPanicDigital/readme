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

For migrations we now user [Phinx](https://phinx.org/). 
[Docs for Phinx](http://docs.phinx.org/en/latest/) are really detailed and full of usefull info and shortcuts


#### Database Init

To init database setup connection detail in `app/config/parameters.neon` (template `app/config/parameters.neon.dist`) and run:
```bash
php www/index.php dbal:import database.sql 
```

#### Define Migrations

Please read the manual for Phinx to understand it's capabilities. Please use `change()` method to write your migrations if possible, otherwise implemetn bot `up()` and `down()` methods.
More info can be found at [creating migrations](http://docs.phinx.org/en/latest/migrations.html#creating-a-new-migration).

Basically, to generate a migration you have to run followint command in root dir of application:
```bash
php vendor/bin/phinx create [MigrationName]
```
[MigrationName] -> Migration name have to be descriptive name of what migration does and it have to be written in camel case.
Afterwards, the new migration with date prefix and camel-case name can be found in `migrations/` folder; 

The most common reason to create new migrations is, when you create or edit some entity.
To retrive changes in schema against current database state run:
```bash
php www/index.php orm:schema-tool:update --dump-sql
```
#### Run Migrations

To run new migrations, run: 
```bash
php vendor/bin/phinx migrate
```

To rollback to previous state:
```bash
php vendor/bin/phinx rollback
```

# Git flow

In our projects we use enhanced git system name git flow. Every project should be a git flow repo (you can turn classic repo into git-flow enabled one via command `git flow init`)
For more informations about git flow, installation process and usage visit `https://danielkummer.github.io/git-flow-cheatsheet/`
Our branches names are separated by `-` (example: `feature-new-feature` or `realease-v1.3.0`). There is no "version prefix" (one of `git init` questions), but we add it manually.

# Deploy
To be able to deploy you have to be allowed to access servers you want to deploy to (be able to log in via ssh).

## Legacy
For deploy of legacy projects (projects with older `dep-deployer` than `^5.0`) use following command to run deploy from folder `.deploy`
```bash
php ../vendor/bin/dep deploy
```

## Basic deploy
We distinguish 3 types of deploys:

### Staging (a.k.a Development)
(usage of speical params : [DOCS - Branch param](https://deployer.org/docs/configuration#branch))
```bash
dep deploy [--branch=feature-featureName] [--tag="v0.1"] [--revision="5daefb59edbaa75"]
```

### Production
```bash
dep deploy production [--branch=feature-featureName] [--tag="v0.1"] [--revision="5daefb59edbaa75"]
```

### Local
```bash
dep local local [--branch=feature-featureName] [--tag="v0.1"] [--revision="5daefb59edbaa75"]
```

## Mics
#### Help
Deployer itself has built-in support for help for commands:
```bash
dep help deploy
```

#### Config
If you want to see your current hosts setup for deployment run:
```bash
dep config:hosts 
```

#### Available tasks
If you want to see all tasks that are available for deploy run:
```bash
dep list
```

#### Update
To update your deployer to most recent version run:
```bash
dep self-update --upgrade 
```
