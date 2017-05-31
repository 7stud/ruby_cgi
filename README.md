You didn't say what server you were using, so I created one with ruby.

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

For that server, you need to create a subdirectory named `public_html` in whatever directory contains the file  `webrick_server.rb`.  In the directory `public.html`, add these files:

page2.htm:
```html
<!doctype html>
<html>
  <head>
    <title>Ruby CGI example</title>
  </head>
  <body>
    <form method="POST", action="/my_prog2.cgi">
      <label for="phone">Digits:</label>
      <input type="tel" name="phone">
      <input type="submit" value="Submit">
    </form>
  </body>
</html>
```

Note that the path used for the action attribute does not specify `public_html`.

my_prog2.cgi:
```ruby
#!/usr/bin/env ruby

require 'cgi'
cgi = CGI.new
puts "Content-type: text/html\n\n"
puts "<html><body><div>You submited: #{cgi['phone']}</body></html>"
```

Make the cgi program executable:

    $ chmod a+x my_prog2.cgi
    
1) Start the server:  
```
$ r webrick_server.rb 
[2017-05-31 12:37:48] INFO  WEBrick 1.3.1
[2017-05-31 12:37:48] INFO  ruby 2.4.0 (2016-12-24) [x86_64-darwin14]
Listening on port: 12500
[2017-05-31 12:37:48] INFO  WEBrick::HTTPServer#start: pid=2128 port=12500
```

2) In your web browser, enter the url: http://localhost:12500/page2.htm

3) Enter something in the input field, then click submit, and you should see something like:
    
    `You submited: 456-7890`
