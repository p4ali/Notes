Web programming really should be as simple as Sinatra.

I've been using Jave like J2EE, then Spring; and Ruby Rails, but feels Sinatra really simplify the web programming techincally and mindly.

Here is what I will choose for next web project:
``` ruby
if static_language then
  spring_boot
else
  if java_script then
    nodejs
  elsif ruby
    sinatra
  else
    ...
  end
end
```

## Rack
Rack provides a modular and adaptable interface for developing web applications in Ruby. By wrapping HTTP requests and responses it unifies the API for web servers, web frameworks, and software in between (the so-called middleware) into a single method call.

Rack provides a minimal interface between webservers supporting Ruby and Ruby frameworks.

To use Rack, provide an `app`: an object that responds to the `call` method, taking the environment hash as a parameter, and returning an Array with three elements:

* The HTTP response code
* A Hash of headers
* The response body, which must respond to each

```ruby
# my_rack_app.rb
 
require 'rack'
 
app = Proc.new do |env|
    ['200', {'Content-Type' => 'text/html'}, ['A barebones rack app.']]
end
 
Rack::Handler::WEBrick.run app
```

To run rack: ` rackup config.ru`
```ruby
# config.ru
 
run Proc.new { |env| ['200', {'Content-Type' => 'text/html'}, ['get rack\'d']] }
```

## Sinatra
Sinatra is a *domain-specific language* for building websites, web services, and web applications in Ruby. It emphasizes a minimalistic approach to development, offering only what is essential to handle HTTP reequests and deliver responses to clients. Sinatra rides on Rack.
* Built for speed
* Minimalism
* Low memory usage

Sinatra applications can also be embedded into other Ruby web applications, packaged as a gem for wider distribution, and so on.
* Sinatra is not a framework.
* Sinatra does not implement MVC. 
* Maintained by Heroku

```ruby
# simple.rb
require 'rubygems'
require 'sinatra'

get '/' do
  "Hello World"
end
```
To run: 
```bash
$ ruby dimple.rb
[2015-01-08 10:40:00] INFO  WEBrick 1.3.1
[2015-01-08 10:40:00] INFO  ruby 1.9.3 (2014-11-13) [x86_64-darwin13.1.0]
== Sinatra/1.4.5 has taken the stage on 4567 for development with backup from WEBrick
[2015-01-08 10:40:00] INFO  WEBrick::HTTPServer#start: pid=74612 port=4567
^C== Sinatra has ended his set (crowd applauds)
^C[2015-01-08 10:40:06] INFO  going to shutdown ...
[2015-01-08 10:40:06] INFO  WEBrick::HTTPServer#start done.
```

## General App structure
```bash
tree
.
├── Gemfile # gems dependency: no version
├── Gemfile.lock # gems with version
├── README.md
├── Rakefile # build and test
├── config.ru # rackup configuration. 
├── dashboard.rb # main app
├── lib
│   └── helpers.rb # helper
├── public
│   ├── favicon.ico # icon show on browser tab
│   ├── images # images used in app
│   │   ├── blue.png
│   │   └── yellow.png
│   ├── javascripts
│   │   └── dashboard.js # javascript in each webpage
│   └── stylesheets
│       └── main.css # cascade style sheet
├── spec
│   ├── dashboard_spec.rb # test
│   └── spec_helper.rb # test helper
└── views # all pages
    ├── dashboard.erb # main page
    ├── layout.erb # ruby will call this to generate each page
    └── table.erb # table page
```
The ruby will generate all pages as following:
```ruby
class Dashboard < Sinatra::Base
  register Sinatra::AdvancedRoutes

  set :public_folder => "public", :static => true

  get '/' do
    erb :dashboard, :locals => { :services_prod => @@services_prod, :services_stag => @@services_stag, :services_ext => @@services_ext }
  end
  
  get '/status' do
    content_type :json
    {
        :status => "100",
        :service_name => "A fake service",
        :date_checked => Time.now.inspect,
        :other_data => "this is fake"
    }.to_json
  end
end
```
* `get /` method in dashboard.rb call `erb :dashboard, :locals => { :services_prod => @@services_prod, :services_stag => @@services_stag, :services_ext => @@services_ext }`
* the `erb` will call `layout.erb`, pass page (e.g., `dashboard.erb`) as parameter
* return generated html page as get body
* `get /status` will return json string as body and not UI.

### Syntax
see [http://stackoverflow.com/questions/17215993/nesting-layouts-in-sinatra](http://stackoverflow.com/questions/17215993/nesting-layouts-in-sinatra)
```
erb :pageTemplate executes layout.erb, where yield executes pageTemplate
```

