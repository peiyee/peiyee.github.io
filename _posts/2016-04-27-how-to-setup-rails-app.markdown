---
layout: post
permalink: /:title
title:  "How to create a rails app"
date:   2016-04-27 17:38:34 +0800
categories: jekyll update
---

## Prerequisite
1) Install rvm

	gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
	\curl -sSL https://get.rvm.io | bash -s 

2) Install ruby

	rvm install ruby-version
	rvm use ruby-version

3) Install rails

	gem install rails
	gem install bundler

4) Install nodejs

	sudo apt-get update
	sudo apt-get install nodejs

5) Install qt5(optional)

Install this if you want to use capybara. 

Reference: [https://github.com/thoughtbot/capybara-webkit/wiki/Installing-Qt-and-compiling-capybara-webkit#debian-testing-unstable-stable-wheezy-ubuntu](https://github.com/thoughtbot/capybara-webkit/wiki/Installing-Qt-and-compiling-capybara-webkit#debian-testing-unstable-stable-wheezy-ubuntu)

	sudo apt-get install qt5-default libqt5webkit5-dev gstreamer1.0-plugins-base gstreamer1.0-tools gstreamer1.0-x

6) Install imagemagick(optional)

Install this if you want to use imagemagick 

Reference: [https://help.ubuntu.com/community/ImageMagick](https://help.ubuntu.com/community/ImageMagick)

	sudo apt-get update
	sudo apt-get install imagemagick --fix-missing

7) Install postgresql(optional)

Install this if you want to use Postgresql as your app database

	sudo apt-get update
	sudo apt-get install postgresql postgresql-contrib
	sudo apt-get install libpq-dev (important for pg gem)

Create a user with superuser access for your postgresql

	sudo -u postgres createuser username -s

---

## Create a New Rails App

1) Create a new app

	rails new <app-name> --database=<database-name>
	eg: rails new quora --database=postgresql

2) Specify ruby version in `Gemfile`, eg: `ruby '2.2.1'`

3) Create a new database

	rake db:create

4) Setup rspec, shoulda, and capybara(selenium driver)
Add following gems to 'Gemfile'

	gem 'puma'
	  
	group :development, :test do
	  gem 'guard'
	  gem 'guard-rspec'
	  gem 'guard-puma'
	  gem 'rspec'
	  gem 'rspec-rails'
	  gem 'shoulda-matchers'
	  gem 'shoulda-callback-matchers'
	  gem 'capybara'
	  gem 'selenium-webdriver'
	  gem 'capybara-webkit'
	end

Then, in the terminal bundle install and initialize rspec and guard

	bundle install
	rails generate rspec:install
	guard init rspec
	guard init puma

Place the Shoulda Matcher configuration in `spec/rails_helper.rb`:

	Shoulda::Matchers.configure do |config|
	  config.integrate do |with|
	    with.test_framework :rspec
	    with.library :rails
	  end
	end

Add this line to `spec/test_helper.rb`:

	require 'capybara/rails'

## Terminal command
1. To start **server**, `rails s`
2. To start **console**, `rails c`
3. To generate/destroy **model**, `rails g/d model <model-name> <attributes>`
4. To generate/destroy **migration**, `rails g/d migration <migration-name> <attributes>`
5. To start guard, `guard`

## Note
Rails will auto generate `README.rdoc`, change this file name to `README.md` in order to use Github markdown.