# How to Create Self Signed SSL 
````bash
openssl req -newkey rsa:2048 -new -nodes -keyout key.pem -out csr.pem
openssl x509 -req -days 365 -in csr.pem -signkey key.pem -out server.crt
````
# How to create an HTTPS server in Node.js
````javascript
var https = require('https');
var fs = require('fs');

var options = {
  key: fs.readFileSync('./key.pem', 'utf8'),
  cert: fs.readFileSync('./server.crt','utf8')
};

https.createServer(options, function (req, res) {
  res.writeHead(200);
  res.end("Welcome to Node.js HTTPS Servern");
}).listen(443, "127.0.0.1");

// Redirect from http port 80 to https 443
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(301, {'Location': "https://"+ req.headers['host'] + req.url});
  res.end();
}).listen(80, "127.0.0.1");

console.log('Server running at http://127.0.0.1:80/');
````
# Reference:
1. Node.js https pem error: routines:PEM_read_bio:no start line
https://stackoverflow.com/questions/22584268/node-js-https-pem-error-routinespem-read-biono-start-line
2. Automatic HTTPS connection/redirect with node.js/express
https://stackoverflow.com/questions/7450940/automatic-https-connection-redirect-with-node-js-express
