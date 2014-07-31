# Newrelic

Loginto heroku, and click raymond app, click the newrelic, will open page for monitoring the transactions, where you will
see the most time-consuming method call.

# Setup

heroku config:set NEW_RELIC_APP_NAME=raymond --app aymond-production
heroku config:set NEW_RELIC_LICENSE_KEY=b407328df2bfc329ca3229d6781324dbc6a578db --app aymond-production
heroku config:set NEW_RELIC_APP_LOG=stdout --app aymond-production
