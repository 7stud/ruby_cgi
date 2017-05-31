You didn't say what server you were using, so I created one with ruby:

webrick_server.rb:
```ruby
require 'webrick'
include WEBrick

port = 12_500
dir = Dir::pwd 

#The default server will serve cgi scripts out of the DocumentRoot.
server = HTTPServer.new(
  :Port           => port,
  :DocumentRoot   => "./public_html",  #Regular files served and .cgi files executed out of this dir
)

puts "Listening on port: #{port}"
Signal.trap('SIGINT') { server.shutdown }  #Ctrl+C will shutdown the server
server.start
```

For that server, you need to create a subdirectory named `public_html` in whatever directory contains the file  `webrick_server.rb`.  In the directory `public.html`, put this file


#!/usr/bin/env ruby
#By default, this script won't execute in this directory 

require 'cgi'
cgi = CGI.new
puts "Content-type: text/html\n\n"
puts "<html><body><div>You submited: #{cgi['phone']}</body></html>"
