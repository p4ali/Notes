## Run Dev on Your machine
* fetch raymond from pdnet
 * `git clone git@pdnet.perforce.com:frontend`
 * `git submodule update`
 * `cd atlas; git co master; git pull`
* fetch atlas-config
 * `git clone git@pdnet.perforce.com:atlas-raymond-config`
* fetch sprout-wrap
 * `git clone git@pdnet.perforce.com:sprout-wrap`
* run sprout-wrap
 * `gem install bundler`
 * `bundle`
 * `bundle exec soloist`
* install rbenv and ruby
 * `brew update; brew install rbenv; brew install ruby-build`
 * `rbenv install 2.1.1`
 * `rbenv rehash`
 * `ruby --version`
* install capistrano
 * `gem install capistrano`
 * `gem update --system`
 * `bundle install --system`
* start atlas vbox
 * `cd atlas; script/valgrant-env dev up`
 * wati about 20 minutes
* provisioning and start atlas
 * `cap dev dev:link; cap dev sanity[test1]`
* run rake tests (backend)
 * `cd atlas; script/rspec`
* run rake tests (frontend)
 * `cd raymond; rake`
 * wait 40 minutes
* start sidekiq
 * `rake sidekiq:start`
 * see http://localhost:3000/sidekiq
* install mailcatcher
 * `cd raymond; gem install mailcatcher`
* start mailcatcher
 * `cd raymond; mailcatcher`
* start raymond
 * `rails s`


## Deploy and test
```bash
#cap vcloud deploy
#cap vcstage deploy
#cap vcloud sanity[test1]
#cap vcloud atlas:restart
#cap vcloud atlas:check

#ssh-add ~/.ssh/id_rsa ## This enable key forwarding to access pdnet form remote using local authenticaton/ssh agent

# run deploy and check
cap dev dev:link
# run sanity check
cap dev sanity[test1]
```
