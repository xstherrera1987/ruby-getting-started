# ruby-getting-started

A barebones Rails app, which can easily be deployed to Heroku and run within Vagrant.

This application supports the [Getting Started with Ruby on Heroku](https://devcenter.heroku.com/articles/getting-started-with-ruby) article - check it out.

This application has been forked from [Heroku's original](https://github.com/heroku/ruby-getting-started) example app in order to work out of the box with [Vagrant's Heroku Box](https://atlas.hashicorp.com/lazygray/boxes/heroku-cedar-14).

## Setup
1. install Vagrant if not already done
2. download and initialize the vagrant box: `vagrant init lazygray/heroku-cedar-14`
3. configure network (eg: create private network or forward ports). examples below.
	* private network: edit `Vagrantfile`, add `config.vm.network "private_network", ip: "10.0.0.0"`
	* forward one port: edit `Vagrantfile`, add `config.vm.network "forwarded_port", guest: 3000, host: 5000, auto_correct: true`
4. start box: `vagrant up`
5. connect to it: `vagrant ssh`
6. add some missing OS packages:
	* Ruby 2.2.0: 
		* `rbenv install 2.2.0`
		* `echo '2.2.0' > /home/vagrant/.rbenv/version`
		* `gem install bundler`
7. configure access to Postgresql and restart it:
	* `sudo vi /etc/postgresql/9.4/main/pg_hba.conf`
		* convenient but insecure method:
		* change the line `local all postgres peer` to `local all all trust`
	* `sudo service postgresql restart`
8. clone this repo: `git clone https://github.com/xstherrera1987/ruby-getting-started.git`
9. nav to the project directory `cd ruby-getting-started`
10. setup app (same as below): `bundle install && bundle exec rake db:create db:migrate`
11. start server: `rails server`
12. (optional) view from host OS: open browser to `http://localhost:5000`
13. download heroku-toolbelt (_same as original_)
	* `echo "deb http://toolbelt.heroku.com/ubuntu ./" > /etc/apt/sources.list.d/heroku.list`
	* `wget -O- https://toolbelt.heroku.com/apt/release.key | apt-key add -`
	* `sudo apt-get update`
	* `sudo apt-get install -y heroku-toolbelt`
14. deploy to heroku (_same as original_)
	* `heroku login`
	* `heroku create`
	* `git push heroku master`
	* ... _etc_

## Running Locally

Make sure you have Ruby installed.  Also, install the [Heroku Toolbelt](https://toolbelt.heroku.com/).

```sh
$ git clone git@github.com:heroku/ruby-getting-started.git
$ cd ruby-getting-started
$ bundle install
$ bundle exec rake db:create db:migrate
$ foreman start web
```

Your app should now be running on [localhost:5000](http://localhost:5000/).

## Deploying to Heroku

```sh
$ heroku create
$ git push heroku master
$ heroku run rake db:migrate
$ heroku open
```

## Docker

The app can be run and tested using the [Heroku Docker CLI plugin](https://devcenter.heroku.com/articles/introduction-local-development-with-docker).

Make sure the plugin is installed:

    heroku plugins:install heroku-docker

Configure Docker and Docker Compose:

    heroku docker:init

And run the app locally:

    docker-compose up web

The app will now be available on the Docker daemon IP on port 8080.

To work with the local database and do migrations, you can open a shell:

    docker-compose run shell
    bundle exec rake db:migrate

You can also use Docker to release to Heroku:

    heroku create
    heroku docker:release
    heroku open

## Documentation

For more information about using Ruby on Heroku, see these Dev Center articles:

- [Ruby on Heroku](https://devcenter.heroku.com/categories/ruby)

