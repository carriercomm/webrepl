About
=================
If you're familiar with [node.js](http://nodejs.org) then you're familiar with the provided REPL. You can embed a REPL in your programs and make it available via tcp or unix sockets so that you can connect to a long running node.js program and play around with it on a command line. Webrepl takes the same idea but makes the repl available via an interactive web page so that you can have all the fun of using a repl right in your web browser. 

* Node.js repl for your process in your browser
* Tab completion is included! 
* Command history via up and down arrows
* Webrepl also makes the properties in your context accessible via restful http calls. 
* UI inspired by http://search.npmjs.org/
* Optional http authentication

![See a Screenshot](https://github.com/mmattozzi/webrepl/raw/master/doc/webrepl.png)

Requires: Node v0.8.0 or higher (for webrepl version 0.4.7+). 

Installation
=================

    npm install webrepl
    
Or just dump all the files into your project's directory. Node module [http-digest](https://github.com/thedjinn/node-http-digest) 
required for auth. This will be fetched by npm, but you will have to fetch it yourself if you're not using npm and want auth.

Usage
=================

    var webrepl = require('webrepl');
    webrepl.start(8080);

Then point your browser to http://localhost:8080 and have fun!

You can provide context variables just like the regular repl:
    
    var webrepl = require('webrepl');
    var foo = { 'bar': 1, 'day': new Date() };
    webrepl.start(8080).context.foo = foo;

HTTP Authentication can be set (uses http digest authentication):

    var webrepl = require('webrepl');
    var options = { 'username': 'user', 'password': 'password' };
    webrepl.start(8080, options);
    
Available options:

    var options = {
       'username': 'username for http authentication, password must also be set',
       'password': 'password for http authentication, username must also be set',
       'hostname': 'hostname to listen on. ex: localhost, 192.168.0.1, etc'
    }
    
You can also access context variables via HTTP, for example: 

    ~ mmattozzi$ curl -i "http://localhost:8080/context/foo"
    HTTP/1.1 200 OK
    Content-Type: application/json
    Connection: keep-alive
    Transfer-Encoding: chunked

    {"bar":1,"two":"dos","today":"2011-02-15T05:33:57.672Z"}
    
    ~ mmattozzi$ curl -i "http://localhost:8080/context/process.pid"
    HTTP/1.1 200 OK
    Content-Type: application/json
    Connection: keep-alive
    Transfer-Encoding: chunked

    33814
    
Security Note
=================

Webrepl can be used to do all sorts of harm to the host system using the require keyword. Think twice, then a 
few more times before exposing webrepl to the world. Optional auth can be used to prevent unwanted access,
but the connection is still insecure/unencrypted. 

