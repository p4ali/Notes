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
Sinatra is a *domain-specific language* for building websites, web services, and web applications in Ruby. It emphasizes a minimalistic approach to development, offering only what is essential to handle HTTP reequests and deliver responses to clients.
* Built for speed
* Minimalism
* Low memory usage

Sinatra applications can also be embedded into other Ruby web applications, packaged as a gem for wider distribution, and so on.
* Sinatra is not a framework.
* Sinatra does not implement MVC. 
* Maintained by Heroku
