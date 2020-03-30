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



grpc was built by google in 2015.

grpc using http2 and protocol buffer as message format.



### Client Server Communication

- Soap , Rest , GraphQL
- WebSocket , SSE 
- Raw TCP



Request Response

SOAP - client need sdk to access the server , schema was XML.

Rest - Representational state transfer ,  transfer in most case data by JSON

​			as JavaScript rise because usually serve JSON format.

​			communicate via HTTP , don't need to worry about schema.

GraphQL - Rest on steroids , give the client query language to API and then minimize number of request to 				server & gets only the data we interested in.



Interactive Communication

WebSocket - bi-directional communication between server and client. 
						close tcp connection only when needed.

SSE - server send events to client.



Raw TCP - protocol upon TCP , define your own protocol.



when writing server side in rest for example the client can be 

- mobile/web app - client that fetch the data from api and give it to the user in this case the platform 							    is library of the api.

  ​								notice that the browser is the biggest HTTP library client and he is responsible for 

  ​								establish communication  with server , handle tls , make sure server support HTTP2 if 								not go to HTTP1 , handle stream , actually he is doing everything for you

- another service that access your service - the service that consume the data is the library , everything is decouple if you specify that the client talks with the server with http1 and you want to change it to HTTP2 you need hard coded change it on the client library , hard to maintain , need always add feature to the client. 



gRPC gives client libraries to all popular languages.

using HTTP/2 as hidden implementation.

Message Format - protocol buffers.



when the world will be moved to HTTP3 you will just need to update client library for clients.

when you receive the data as bytes you des it to your language model 



gRPC Modes :

- Unary RPC - Request Response.
- Server Streaming RPC - client excepting a lot of data come from server , stream of data.
- Client Streaming RPC - client stream data to the server.
- Bidirectional streaming RPC - server communicate with client and vice versa.



Coding Time



first we create our protobuf

```todo.proto```

```protobuf
syntax = "proto3"; //version of protobuf

package todoPackage; //declare schema name

service Todo {
    rpc createTodo(TodoItem) returns ( TodoItem ); //specify method called createTodo
    rpc readTodos(noValueItem) returns ( TodoItems ); //specify method called readTodo
    rpc readTodosStream(noValueItem) returns ( stream TodoItem ); //stream the results
}

message noValueItem {}

message TodoItem {
    int32 id = 1; //first argument always be id
    string text = 2; ////second argument always be text
}

message TodoItems {
    repeated TodoItem items = 1; //repeated is equivalent to array
}
```



```server.js```

```javascript
const grpc = require("grpc");
const protoLoader = require("@grpc/proto-loader"); //import proto compiler

const pacakgeDef = protoLoader.loadSync("todo.proto",{}); // load the proto file
const grcpObject = grpc.loadPackageDefinition(pacakgeDef); 

const todoPacakge = grcpObject.todoPackage; //get our todo schema

const server = new grpc.Server(); 

server.bind("0.0.0.0:40000",  grpc.ServerCredentials.createInsecure());

server.addService(todoPacakge.Todo.service,
                  {
                    "createTodo" : createTodo, //specify method createTodo
                    "readTodos" : readTodos, //specify method readTodos
                    "readTodosStream" : readTodosStream //specify method readTodosStream
                  });

server.start();

const todos = [];
//call is the client
//callback how do you react
function createTodo(call,callback){
    const todoItem = {
        "id" : todos.length +1,
        "text" : call.request.text
    }
    todos.push(todoItem);

    callback(null,todoItem) //free the client , close tcp connection
}

function readTodos(call,callback){
    callback(null,{"items": todos})
}

function readTodosStream(call,callback){
    todos.forEach(t=>call.write(t)); //stream the data to client
    call.end(); //close tcp connection
}
```

```client.js```

```javascript
const grpc = require("grpc");
const protoLoader = require("@grpc/proto-loader"); //impot proto compiler

const pacakgeDef = protoLoader.loadSync("todo.proto",{}); // load the proto file
const grcpObject = grpc.loadPackageDefinition(pacakgeDef); 

const todoPacakge = grcpObject.todoPackage; //get our todo schema

const client = new todoPacakge.Todo("localhost:40000",grpc.credentials.createInsecure()); 

client.createTodo({
    "id" : -1,
    "text" : process.argv[2]
}, (err,response)=>{
    console.log(`Recived from server ${JSON.stringify(response)}`)
});


const call = client.readTodosStream();
call.on("data" , item => {
    console.log(`recived data as stream from server ${JSON.stringify(item)}`)
})

call.on("end" , item => {
    console.log(`Finish Get Stream`)
})
```



**Notice that the client can be written in each supported language , you just need to have the```todo.proto``` file**

 

## Pros & Cons

Pros :

- fast because using HTTP2 and protobuf.
- One client library , rest api required library to support this JavaScript (axios , fetch ,request);
- cancel request because HTTP2
- supproted to lot languge.

Cons:

- Schema , you force a schema.
- Thick client -> you compile proto file each running.
- proxies , not every proxy support this (layer 7).
- young technology.
- no native support for browser.
- Unary RPC , simple request response timeout how much time do you have to wait ? (ms=micro service) ms1 call to ms2 to ms3 you have to manage chain of timeout , the soulution using pub/sub let your ms listen to the required data.



