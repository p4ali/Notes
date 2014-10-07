I normally use the interactive console for testing code I am changing, launch it with
```
jruby -S bundle exec jirb
```
then 
```
require 'app.rb'
```

I normally test api endpoints separately with curl or with the request framework in rspec
you need to reload apache or passenger when you change things and it gets a bit time consuming
if you set the callback_url to http://uploader.atlas.dev/test/callback, it will write the callback to tmp/callback_payload.json
This is where most of the logs end up:
```
/srv/atlas/log/*.log
/var/lib/perforce/your.project.atlas.dev/log/*.log
/var/log/apache2/* (must be root)
```

Run just the revert integration tests with
```
cd /srv/atlas/uploader; 
sudo -uperforce jruby -S bundle exec rspec spec/integration/edit/revert_spec.rb
```
