
"scripts": {
    "mocks": "node mocks.js & ng serve"
}

const http = require('http');

const mock = [
    { id: 1, name: 'iPhone', price: 800 },
    { id: 2, name: 'iPad', price: 650 },
    { id: 3, name: 'iWatch', price: 750 }
];

const server = http.createServer((_, res) => {
    
    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Methods', 'GET, POST');
    res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify(mock));

});

server.listen(5000, () => console.log('Fake server is listening on port 5000'));
