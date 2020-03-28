# ProtoBuf

Let's start talk about json.

we have employee that have

```json
{
    "id" : 1001,
    "name" : "Aviad",
    "Salary" : 1000
}
```



let's assume we want to create an array : 

```json
[
    {
    "id" : 1001,
    "name" : "Aviad",
    "Salary" : 1000
	},
    {
    "id" : 1002,
    "name" : "Amir",
    "Salary" : 3000
	},
     {
    "id" : 1003,
    "name" : "Reut",
    "Salary" : 2500
	}
]
```



you can see that a lot of fields are unnecessary and repeat on them self.

"id", "name","salary", {}.



protobuf come to make this as schema and give up on this unnecessary things.



to use protobuf we have to create ```employees.proto``` of our file.

```protobuf
syntax = "proto3";

message Employee {
    int32 id = 1; //number one property
    string name = 2;
    float salary = 3;
}

message Employees {
    repeated Employee employees = 1;
}
```

then using protoc to compile the schema to our language.

```proto/bin/protoc --js_out=import_style=commonjs,binary:. employees.proto``` 

```javascript
const Schema = require('./employees_pb');

const ahmed = new Schema.Employee();
ahmed.setId(1002);
ahmed.setName("Ahmed");
ahmed.setSalary(9000);

const rick = new Schema.Employee();
rick.setId(1003);
rick.setName("Rick");
rick.setSalary(5000);


const employees = new Schema.Employees();
employees.addEmployees(hussein);
employees.addEmployees(ahmed);

const bytes = employees.serializeBinary();
const employees2 = Schema.Employees.deserializeBinary(bytes);

```



now this will be generated as bytes.

pros : 

- schema. 

- size.
- common to all languages.

cons : 

- using external things like protoc.



grpc is build upon this.



# HTTP 

## Http 1.1

Browser open tcp for each connection , if you would like to get from server

- GET /index.html
- GET /main.css
- GET /index.js
- GET /img1.jpg
- GET /img2.jpg

this will be sync or open 5 tcp connections.



## Http 2.0

Each things will be streamed on same tcp connection.

By help of multiplexing  , each thing will be tagged on his content 

or each connection , if you would like to get from server

- GET /index.html (stream 1)
- GET /main.css (stream 2)
- GET /index.js (stream 3)
- GET /img1.jpg (stream 4)
- GET /img2.jpg (stream 5)

couple response on same tcp connection  !!!



problemtic when server is using http2 but proxy using http1

# WebSocket

http requests open tcp connection between client and server.

when the data is servered to client the tcp connection is being killed.



web sockets open tcp connection and close it when needed , the client can communicate with server and server with client till the connection is open.

web socket connection :

1. client send request to server.
2. server switch protocol to (Status 101) and then the connection between them is open

this called handshake.



why do we want full duplex bidirectional communication 

- Chatting 
- Live feed
- multi-player gaming 
- showing client progress of process.



Let's get our hand dirty 

```javascript
npm i --save websocket
```

```javascript
const http = require("http");
const WebSocketServer = require("websocket").server;
let connection = null;

const port = 8080;
const httpServer = http.createServer((req,res)=>{
    console.log("We have recived a request");
})

const websocket = new WebSocketServer({
    "httpServer" : httpServer
})

websocket.on("request",request => {
    connection = request.accept(null,request.origin);
    connection.on("open",()=>console.log("Opened!!!"));
    connection.on("close",()=>console.log("Closed!!!"));
    connection.on("message",message=>console.log(`recived ${message}`));
});

setInterval(() => {
    connection.send('hello aviad')
}, 5000);
httpServer.listen(port, ()=> console.log(`Server listening port ${port}`));
```

we'll send the client message each 5000 ms.



we can access to ws from browser without any setup.

to create connection run

```javascript
const ws = new WebSocket("ws://localhost:8080")
```

to decide how to react to messages run 

```javascript
ws.onmessage = message => console.log(`hello ${message.data}`)
```

to send message write 

```javascript
ws.send('your message')
```

to close the connection run

```javascript
ws.close()
```



## WS Pros & Cons

Pros :

- Full-duplex (no polling)
- HTTP compatible
- Firewall friendly

Cons :

- Proxying is tricky
- challenging with timeout and load balancer.
- Difficult horizontally scale



Alternative to websocket

- Long polling
- EventSource
- Server Send Events

# GRPC



 