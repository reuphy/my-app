pacakge.json 
"proxy-fake-backend": "ng serve --proxy-config proxy.conf.json",
"fake-backend-server": "node mocks.js",

-------------------------------------------------
proxy.conf.json
{
    "/mock/*": {
      "target": "http://localhost:5000",
      "pathRewrite": {"^/mock": ""},
      "secure":"false"
    }
}

--------------------------------------------

const http = require('http');

const mock = [
    { id: 1, name: 'iPhone', price: 800 },
    { id: 2, name: 'iPad', price: 650 },
    { id: 3, name: 'iWatch', price: 750 }
];

mocks.js

const server = http.createServer((req, res) => {
    
    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Methods', 'GET, POST');
    res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

    if(req.url.includes('people')) {
        res.writeHead(200, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify([
            { name : 'jack' },
            { name : 'max' },
        ]));
    }
    else {
        res.writeHead(200, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify(mock));
    }
});

server.listen(5000, () => console.log('Fake server is listening on port 5000'));
