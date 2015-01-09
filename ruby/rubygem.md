## Gem
Gem is source code and libraries to share. It has version, name(must be unique when you push) and platform.

To create and publish a simple gem, see [http://guides.rubygems.org/make-your-own-gem/](http://guides.rubygems.org/make-your-own-gem/)
To see where gem is installed, use following command:
```bash
$ tree
.
├── hello_p4ali-1.0.0.gem
├── hello_p4ali.gemspec
└── lib
    └── hello_p4ali.rb
$ gem build hello_p4ali.gemspec
$ gem install hello_p4ali -V --backtrace
Installing gem hello_p4ali-1.0.0
Using local gem /Users/ali/ruby1.9.3-with-tk/homebrew/lib/ruby/gems/1.9.1/cache/hello_p4ali-1.0.0.gem
/Users/ali/ruby1.9.3-with-tk/homebrew/lib/ruby/gems/1.9.1/gems/hello_p4ali-1.0.0/lib/hello_p4ali.rb
Successfully installed hello_p4ali-1.0.0
1 gem installed
```
### Gem with extension
Like JNI, Gem can also wrap native library. see [http://guides.rubygems.org/gems-with-extensions/](http://guides.rubygems.org/gems-with-extensions/)

## RubyGems
RubyGems is a package manager for the Ruby programming language that provides a standard format 
for distributing Ruby programs and libraries (in a self-contained format called a "gem"), a tool 
designed to easily manage the installation of gems, and a server for distributing them.

## `.ruby-version`
You can always set the ruby version by add `.ruby-version` file in the current folder.
```
$ cat .ruby-version
2.1.2
```
