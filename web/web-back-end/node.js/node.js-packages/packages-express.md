# Packages :: Express

#### 01. Documents

* npm doc : [https://www.npm.js/package/express](https://www.npm.js/package/express)
* express official site : [http://expressjs.com/ko/starter/installing.html](http://expressjs.com/ko/starter/installing.html)

#### 02. Quick start

우선 그냥 해보기.

**HTTP**

{% code-tabs %}
{% code-tabs-item title="App.js" %}
```javascript
var express = require('express'),
    http = require('http'),
    app = express(),
    server = http.createServer(app);


app.get('/', function (req, res) {
  res.send('Hello /');
});


app.get('/world.html', function (req, res) {
  res.send('Hello World');
});


server.listen(8000, function() {
  console.log('Express server listening on port ' + server.address().port);
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

App 실행

node app.js

**HTTPS**

{% code-tabs %}
{% code-tabs-item title="App.js" %}
```javascript
var express = require('express'),
    http=require('http'), 
    https = require('https'),
    app = express(),
    fs = require('fs');
 
var options = { 
    key: fs.readFileSync('key.pem'),
    cert: fs.readFileSync('cert.pem')
};
 
app.use(express.urlencoded());
 
http.createServer(app).listen(80, function(){ 
  console.log("Http server listening on port 80");
});
 
https.createServer(options, app).listen(443, function(){ 
  console.log("Https server listening on port 443");
});
 
app.get('/', function (req, res) { 
    res.writeHead(200, {'Content-Type' : 'text/html'});
    res.write('<h3>Welcome</h3>');
    res.write('<a href="/login">Please login</a>');
    res.end();
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

App 실행

node app.js

**Express-generator**

```bash
sudo yarn install -g express-generator
express --view ejs (view를 담당하는 파일 확장자 지정..)
yarn install
node bin/www
```

