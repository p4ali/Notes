## Run Dev on Your machine
You must have an account at pdnet, and have the ssh key ready in order to use git to check out code.

1. fetch raymond from pdnet
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
 * `cd atlas; script/vagrant-env dev destroy -f; script/valgrant-env dev up`
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
* reset and migrate db
 * `rake db:reset db:migrate`
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

## Update
```bash
# figure out where ruby installed
$ which ruby
/Users/ali/.rbenv/shims/ruby
$ rbenv install 2.1.3
$ bundle install --system --without nothing
```

## Continous Integration
* Helix integration test (end to end, with commandline `cd raymond; ciborg open`) [https://54.186.9.9/job/raymond-rails/](https://54.186.9.9/job/raymond-rails/)
* Atlas test [http://jenkins.bnr.perforce.com/view/helix/job/atlas-ci-master/](http://jenkins.bnr.perforce.com/view/helix/job/atlas-ci-master/) `ssh -i ~/.ssh/id_jenkins_ci.pub perforce@atlas-jenkins-slave2.das.perforce.com` with classical password.
* Uploader teest [http://jenkins.bnr.perforce.com/view/helix/job/uploader-ci-master/](http://jenkins.bnr.perforce.com/view/helix/job/uploader-ci-master/)
