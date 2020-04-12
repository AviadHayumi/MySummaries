# NodeJS Book

nodejs born at 2009 , node allows you run JavaScript out of the browser.

is built upon v8 engine and libuv library that written in C and give you good performance.

In nodejs you don't need to manage threads node do it for you.

the platform is non-blocking , if you need to query and get something from database this is not block your program , the program will continue to run , when the query finished this will call to next operation that will not block to.

Node is built to run anywhere without special setups just need to have Node on the server , this can run on windows , mac ,Linux.

NodeJS is not a language is a platform for JavaScript.



## Modules

What makes node so robust is you ! the community .

Node has a lot of useful modules that can be written by anybody.

what differences node from the code that you familiar in the browser is the `require` we'll figure this later.

Node also comes with built in modules.

For now we'll focus on `fs` module , the module that gives you the ability control file system , read files , etc..



`index.js`

```javascript
const fs = require('fs');

fs.readdir('./',(err,result)=>{
    console.log(result)
});
```

this will give you the exists file/directory of a path.

first argument is path.

second argument is a callback.

the output is 

```bash
[ 'index.js' ]
```

`./` is relevant for where are you in the cli.



let's create a file

`createFile.js`

```javascript
const fs = require('fs');

fs.writeFile('./test.txt', 'Hello World!',(err)=>{
    if(err) throw err;
    console.log('file created succsesfully');
})
```

first argument is path 

second argument is content of file

third is callback that contains err if exists.



[Node docs]: https://nodejs.org/en/docs/	"docs of node , included or defaults modules"



one of the rules of nodejs is that you are add new modules when needed add features to platform.

Usally you don't use built in modules and build with them everything , most of the devlopers use existing modules that diffrent developers have created.



all this modules are exists in npm = Node Packge Manager.

Npm also help us manage our modules this exists in `package.json` that includes :

- list of all required modules.
- version.
- description and meta data about project.
- scripts.

we can create `package.json` by `npm init` or if you are lazy `npm init -y`



to install module we just need to run `npm install <module-name>` 

let's import lodash

`npm i loadash`



if the external moudle use another module npm will import all required module.

the module code exists in directory called `node_modules`



if you want to install all modules that exists in `package.json` run `npm i`.



### Methods sync & async

we have two types of methods 

- async methods - methods that will run in the future , and not block the execution of other things.
- sync methods - methods that block the execution until they will finish.



One of our big targets as node programmers is using sync , or block the execution.



for example we have two kinds of reading directory :

- `readdirSync(path)` - if error will occur we should catch it with try catch block.
- `readdir(path |,options| , error)`



if we would like to transform function that using callback to function using promise we have to :

- require `util` module.
- `const newFunction = util.promisify(old_function)`



for example let's take readdir and change it.

```javascript
const util = require('util');
const fs = require('fs');
const readdir = util.promisify(fs.readdir);

(async ()=> {
    const result = await readdir('.');
    console.log(result);
})()
```

*This is going to work only of methods that have that first argument is an error and second is the result*

if there is more than one result , what will happen is 

all result values will be wrapped in object that key is the argument key and value is the value.



Today there are support in most cases with promises.

You don't have to use promisify

check if module support promises and if yes this is common convention

```javascript
const fs = require('fs').promises;

async function makedir(){
    await fs.makeddir('./name.txt','aviados');
}
```



### Events

events is very important service in NodeJS that allows modules and objects talk to each other easily.

You can communicate with promises or events.

Distributed services used to use events , I/O services used to use promises.

Most core functionality based on events.

Create a server listen to request and response to this is based on events architecture.

```javascript
const EventEmitter = require('events');
const emitter = new EventEmitter();

//register a listner
emitter.addListenr('name-of-the-event' , function(){
    console.log(`Listener Called`
} ) 
//or using
emitter.on('name-of-the-event' , function(){
    console.log(`Listener Called`);
})
    
emitter.emit('name-of-the-event'); //raise an event , produce an event

```





```javascript
const EvnetEmitter = require('events');

// create custom events emmiter
class MyClass extends EventEmitter {};
const myClass = new MyClass(); 

// create listener to events called someEvent
myClass.on('someEvent',()=> {
    console.log('event occurd!');
})

myClass.emit('someEvent'); //call the event
```



this is similar to events in the browser , the small change is that here we create the events.



function that handle react to events that happen used to be called handler.

we can turn down lister by 

```javascript
//insted of this
myClass.on('someEvent',()=> {
    console.log('event occurd!');
})
//use this
myClass.on('someEvent',this.someEventHandler)
```

we can turn off listener by

```javascript
myClass.of('someEvent' , someEventListner)
```

We can listen once and turn off listener by `once`



We can react in various ways to event that happened.

```javascript
const EventEmitter = require('events');

class MyClass extends EventEmitter{
    
    constructor(){
        super();
        this.on('someEvent',this.someEventHandler1);
        this.on('someEvent',this.someEventHandler2);
    }
    someEventHandler1(){
        
    }
    someEventHandler2(){
        
    }
}
```

The order of execution will be by the order of the code , first first , second second.

we can guarantee that handler will be fitst by 

```javascript
myClass.prependListener('someEvenet',this.someEventOtherHandler);
```

we can remove all listeners by

```javascript
myClass.removeAllListeners('someEvent');
```



We can put data when emit

```javascript
myClass.emit('someEvent', 'Helo to you!!')
```

and get it by

```javascript
myClass.on('someEvent', someEventHandler);
//or
myClass.on('someEvent', (args)=>{
    //execution...
});

```



Now I'll create DogEmitter that gonna bark where the is a food

`DogEmitter.js`

```javascript
const EventEmitter = require('events');

class DogEmitter extends EventEmitter{
    constructor(){
        super();
        this.on('food',this.foodHandler);
    }

    foodHandler(foodname){
        console.log(`Woof!! I LIKE ${foodname}`);
    }

    callMe(foodname){
        this.emit('food',foodname || 'Bonzo');
    }
}

module.exports = DogEmitter;
```

`index.js`

```javascript
const DogEmitter = require('./dogEmitter');

const dogEmitter = new DogEmitter();
dogEmitter.callMe('Banana');
dogEmitter.callMe('Meat');
dogEmitter.callMe();
```

Output

```javascript
Woof!! I LIKE Banana
Woof!! I LIKE Meat
Woof!! I LIKE Bonzo
```



### Create HTTP Server

The famous module http that allows us create server/client is extends of events.

Http using net.Server that using EventEmitter.

```javascript
const httpServer = require('http').Server;

class MyServer extends httpServer {
    constructor(port){
        super();
        this.listen(port,()=> console.log(`server listen on port ${port}`));
        this.on('request',this.requestHandler);
    }
    
    requestHandler(req,res){
        res.end('Hello World!');
    }
}

const port = process.argv[2] || 3000;
const myServer = new MyServer(port);
```



connection events occur each time client connect to server

```javascript
this.on('connection',this.connectionHandler)
connectionHandler(client){
    console.log(`client ${client} has connected`)
}
```



now we'll create web server that serves a file

```javascript
const httpServer = require('http').Server;
const fs = require('fs').promises;

class MyServer extends httpServer {
    constructor(port){
        super();
        this.listen(port,()=>console.log(`server listen on port ${port}`));
        this.on('request',this.requestHandler);
    }
   
    async requestHandler(req,res){
        if(req.method == 'GET' && req.url == '/file'){
            const file = await fs.readFile('./file.txt');
            console.log(file)
            return res.end(file);
        } 
        return res.end('hello')
    }
}

const port = process.argv[2] || 3000;
const myServer = new MyServer(port);
```



when our file is big , this is may be problematic because this will stuck the event loop.

in this case using streams is preferred.

 

```javascript
    async requestHandler(req,res){
        if(req.method == 'GET' && req.url == '/file'){
            const stream = fs.createReadStream('./file.txt');
            return res.pipe(res);
        } 
        return res.end('hello')
    }
```





### Create CLI

so we have module that capble of encrypt file

```javascript
const fs = require('fs');
const crypto = require('crypto');

class Encryption {
    constructor(){
        this.algorithm = 'aes-256-ctr';
        this.password = 'Password used to generate key';
        this.key = crypto.scryptSync(this.password,'SOmeSalt',32);
        this.iv = 'myIVstringisnice1'.toString('hex').slice(0,16);
    }

    encrypt(sourceFileName){
        const destinationFileName = sourceFileName + '.encrypted';
        const readStream = fs.createReadStream(sourceFileName);
        const writeStream = fs.createWriteStream(destinationFileName);
        const encryptStream = crypto.createCipheriv(this.algorithm,this.key,this.iv);
        readStream.pipe(encryptStream).pipe(writeStream);
        writeStream.on('finish',this.onEnd)
    }

    onEnd(){
        console.log('Finished!!!');
    }
}

module.exports = Encryption;
```



`process.argv[0]` - place of node.

`process.argv[1]` - place of js that we are working on.

`process.argv[2]` - arg 1 

`process.argv[3]` - arg 2



ok so what I want to do is to run `encrypt file.txt` and get as a result `file.txt.enctypted` that will contains encrypted data. 

`/bin/encrypt.js`

```javascript
#!/usr/bin/env node

const Encrypt = require('../src/encrypt');

const sourceFileName = process.argv[2];

if(!sourceFileName){
    console.log('please give file name')
    return;
}

const encrypt = new Encrypt();
encrypt.encrypt(sourceFileName)
```

`package.json`

```json
"bin": {
    "encrypt": "./bin/encrypt.js",
  }
```

`sudo npm link` 



now this is working.



### Sockets

if we would like to handle bi-directional communication and the real time is very important for us , we can use sockets , in sockets the connection not being closed until the client or server is close it.

then client/server can send messages to each other.

```javascript
const netServer = require('net').Server;

class SocketSever extends netServer {
    constructor(){
        super();
        this.listen('6969');
        this.on('connection',this.connectionHandler)
    }

    connectionHandler(socket){
        console.log(`Someone connected ${socket.remoteAddress}`);
        this.socket = socket;
        this.socket.on('data',this.dataHandler);
        setInterval(() => {
            const currentTime = new Date().getTime().toString();
            this.socket.write(currentTime + '\n');
        }, 500);

        this.socket.write('Weloce to my server');
    }

    dataHandler(typedData){
        console.log(`Typed This ${typedData}`);
    }
}

module.exports = new SocketSever();

```



### Path

when we speify path `./imges` the dot is related to the place we ran the node program. 

if we ran node program from diffrent dir this can occur errors because this wont found the path of `/images` . 

what we can do to handle it is to use `__dirname` that we'll return the path of the file.

for example if we have an

- src
  - modules
    - routes
      - shopRoute.js
    - controllers
      - shopController.js
- public
  - index.html
- app.js



if we will try to get file `index.html` from `app.js` by using `./public/index.html` if we'll run `app.js` from diffrent directory we will not find it.

therefore what we'll do is 

```javascript
const path = require('path');

path.join(__dirname, '../public/index.html')
```



### ENV Variables 

we usually don't want to save sensitive data on our program , and also we want to be flexible.

for example i don't want to save my password on database on the code , and also i want it to be flexible between test,dev,pp,prod environments.

Therefore we'll use environment variables. 

this

```javascript
const connection = mysql.createConnection({
   host : 'localhost',
   user : 'root',
   password : '123456',
   database : 'test' 
});
```

will be replaced with this

```javascript
const connection = mysql.createConnection({
   host : process,env.HOST ,
   user : process,env.USER,
   password : process,env.PASSWORD,
   database : process,env.DATABASE
});
```



