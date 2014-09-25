## Rake and rspec

| Command   | Description |
|-----------|:-------------|
|```bundle exec rspec spec/ --tag dev-triggers``` # or ~atlas |run all tests under spec/ who is annotated dev-triggers:true|
|```rake db:drop db:create db:migrate``` | recreate db |
|```rake db:test:prepare``` | prepare db for test|


## Git
| Command   | Description |
|-----------|:-------------|
|```git lg --all```|show git log|
|```git duet -g al tw```|globally change duet config, i.e., change the ~/.git/config|
|```git reset --soft @^```|reset to previous version| 

## Ciborg
| Command   | Description |
|-----------|:-------------|
|```ciborg ssh```|ssh to ciborg build machine|
|```ciborg open```|Open browser to build machine|

## Heroku
| Command   | Description |
|-----------|:-------------|
|```heroku config```|listing heroku configuration|
|```heroku config:set atlas-staging.cloudns.pw```|change heroku configuration|
|```heroku run bash```|bash environment|
|```heroku run console```|attach irb console to heroku|
|```heroku logs```|show logs|
|```heroku logs -t --app raymond-staging```|show logs lively|

## Pusher

Login to pusher with ali@perforce.com + team password. You can monitor the trigger events

## newrelic

Login to monitor the product performance.

## Sidekiq

Redis supported priority queue

http://localhost:3000/sidekiq
