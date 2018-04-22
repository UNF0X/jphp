
## jphp-httpserver-ext

> Library for create http servers.

### How to use?

0. **See sdk of the main classes**:
- [HttpServer.php](https://github.com/jphp-compiler/jphp/blob/master/exts/jphp-httpserver-ext/src/main/resources/JPHP-INF/sdk/php/http/HttpServer.php).
- [HttpServerRequest.php](https://github.com/jphp-compiler/jphp/blob/master/exts/jphp-httpserver-ext/src/main/resources/JPHP-INF/sdk/php/http/HttpServerRequest.php)
- [HttpServerResponse.php](https://github.com/jphp-compiler/jphp/blob/master/exts/jphp-httpserver-ext/src/main/resources/JPHP-INF/sdk/php/http/HttpServerResponse.php)


1. **Run server with hello world page**.
```php
use php\http\{HttpServer, HttpServerRequest, HttpServerResponse};

// create server at 8888 port, 127.0.0.1 host.
$server = new HttpServer(8888, '127.0.0.1'); // port & host.

// add route with method + path + handler.
$server->route('GET', '/hello-world', function (HttpServerRequest $req, HttpServerResponse $res) {
    $res->contentType('text/html');
    $res->body('Hello, <b>World</b>');
});

// run server.
$server->run();
```
Check it, open [http://localhost:8888/hello-world](http://localhost:8888/hello-world) in your browser.

---

2. **Template routing**.

```php
use php\http\{HttpServer, HttpServerRequest, HttpServerResponse};

// create server at 8888 port, 127.0.0.1 host.
$server = new HttpServer(8888, '127.0.0.1'); // port & host.

// add route with method + path + handler.
$server->route('GET', '/hello/{name}', function (HttpServerRequest $req, HttpServerResponse $res) {
    $name = $req->attribute('name');
    
    $res->contentType('text/html');
    $res->body("Hello, <b>$name</b>.");
});

// run server.
$server->run();
```

Check it, open [http://localhost:8888/hello/YourName](http://localhost:8888/hello/YourName) in your browser.

---

3. **Start server in background**:

```php
$server->runInBackground(); // run in background thread.
```

---

4. **Stop server, check is running**.
```php
if ($server->isRunning()) {
    $server->shutdown();
}
```

4. **Get client data**.
```php
use php\http\{HttpServer, HttpServerRequest, HttpServerResponse};

// create server at 8888 port, 127.0.0.1 host.
$server = new HttpServer(8888, '127.0.0.1'); // port & host.

// add route with method + path + handler.
$server->route('GET', '/client_data', function (HttpServerRequest $req, HttpServerResponse $res) {
    $client_data = '<b>Remove Adress</b> - '.$req->remoteAddress().'<br>'; //Add Remote Adress
    $client_data .= '<b>Headers</b>:<br>'.print_r($req->headers(),1).'<br>'; //Add Headers
    $client_data .= '<b>LocalAddress</b> - '.$req->localAddress().'<br>'; //Add LocalAress
    $client_data .= '<b>LocalPort</b> - '.$req->localPort().'<br>'; // Add LocalPort
    $client_data .= '<b>Cookies</b> - '.print_r($req->cookies(),1).'<br>'; //Add Cookies
    $res->contentType('text/html');
    $res->body("You client data: <br>$client_data"); // Add client data to body
});

// run server.
$server->run();
```
Check it, open [http://localhost:8888/client-data](http://localhost:8888/hello-world) in your browser.
