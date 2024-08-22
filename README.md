const http = require('http');

const mock = [
    { id: 1, name: 'iPhone', price: 800 },
    { id: 2, name: 'iPad', price: 650 },
    { id: 3, name: 'iWatch', price: 750 }
];

const server = http.createServer((req, res) => {
    if (req.url === '/mocks') {
        res.writeHead(200, { 'Content-Type': 'application/json' });
        res.end(JSON.stringify(mock));
    } 
    else {
        res.writeHead(404, { 'Content-Type': 'text/plain' });
        res.end('Not Found');
    }
});

server.listen(5000, () => {
    console.log('Serveur écoute sur le port 5000');
});
