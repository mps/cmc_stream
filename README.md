This code is a series of Rake tasks that launch AWS instances of Wowza Media Server.  Wowza allows you to point a stream at it and broadcast it over the internet.

* make sure bundler is installed, type: `sudo gem install bundler`
* make sure all gems are installed, type: `bundle install`
* run your command, type `rake setup` to launch an instance and `rake stop` to kill the instance
