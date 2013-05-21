# Bitfication
Bitfication powers bitfication.com, a bitcoin trading platform. It is :

* Open Source,
* Based on Ruby on Rails,
* Fully localizable,
* Multi-currency.

# Installation
Linux and Windows setup work well, I found the easiest to be an Ubuntu install

## Linux (Debian flavors)
* Install required packages

        $ sudo apt-get install ruby ruby-dev libssl-dev irb rubygems mysql-server libmysql++-dev build-essential git

## Windows
* Install Ruby and MySQL
* Install Ruby Development Kit (https://github.com/oneclick/rubyinstaller/wiki/development-kit)
* Install rubygems

## Common
* Install the `bundler` rubygem, it will easily manage and compile all the other dependencies

        $ sudo gem install bundler

* Fork project if relevant
* Check out sources with git

        $ git clone https://bitbucket.org/thiagocmc/bitfication.git

* Get into the sources directory

        $ cd bitfication

* Compile and install the required dependencies

        $ bundle

* Log-in to MySQL console and run the following commands. If you are installing a production machine you'll obviously need to pick different credentials. Update the `config/database.yml` file accordingly.

        > CREATE DATABASE `bitcoin-bank_development`;
        > GRANT ALL PRIVILEGES ON `bitcoin-bank_development`.* TO 'rails'@'localhost' IDENTIFIED BY 'rails';

* Run a couple of rake tasks (omit the `RAILS_ENV` option if you're setting up a development environment, Rails will grab the database configuration in the `config/database.yml` file under the right section (development, test, or production)

        $ rake db:migrate RAILS_ENV=production

* Edit config/bitcoin.yml to be able to connect your instance to a bitcoin client, the `config/bitcoin.yml` file contents are self-explanatory, just add a production section if you're deploying on a production server.

* You're good to go! Run the rails server

        $ rails s

Your fresh instance should now be running on `http://localhost:3000/` !

## Production deployment

Usually, Rails applications are deployed in production using nginx or Apache, I'll introduce the Apache option.

The `capistrano` tool is used to automate pretty much every deployment step. Deploying a new version is as easy as typing `cap deploy` in your local command prompt.

To use the `cap` sweetness a couple of extra steps are required : 

* You'll need to fork the project since all your deployment configuration is stored in `config/deploy.rb`, these configs are pulled directly from GitHub when deploying, so go for it, change them to suit your needs.
* Set the remote machine up by typing `cap deploy:setup`
* Log in to the remote machine and create the production configuration files in `{APP PATH}/shared/config/*.yml`, they will be used in production (you don't want your production passwords hanging around on GitHub do you ?)
* Create the remote DB
* Now you can run locally `cap deploy:migrations`, this will update the remote sources and run the migrations on the remote database
* Now you just need to install the `passenger` gem on the remote server which will install an apache module
* Create an apache virtual host and you're good to go.

You'll just need to issue a `cap deploy` locally for any subsequent deployment.

# Contributions
All are welcome, improvements, fixes and translations (the string extraction bounty has been paid).

 * The use of the `Numeric#to_f` method is big no-no, every single numeric that passes through the code should be typed as `BigDecimal`,
 * Bugfixes should include a failing test,
 * Pull requests should apply cleanly on top of `master`, rebase if necessary

# License
AGPL License. Copyright 2010-2011 David FRANCOIS
