![image info](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d9/Node.js_logo.svg/1200px-Node.js_logo.svg.png
)


# What Is It NodeJS ? 
Node build on v8 engine that was built by google and add features to it. 

It allows you run java-script out of the browser.

Engine means takes js code and compiles it to machine code.

v8 is written by c++
nodejs takes v8 and add some features likes : 
- filesystem
- network
- operating system

some feautres that you are familliar from brower not exists in node

node as server :
- acesses to database
- authentication to server
- input validation 
- all the bls

Node js is not only for servers , you can run js anywhere.

## REPL
Read Eval Print Loop

which means run javascript code.

we can do it in 2 ways 
1. just open console and run node some_file.js
2. nice playground for running javascript&node from cli without js file.
- open terminal. 
- write node
- and then whathever you want to run as js code 



# BREIF OF JS 
pardigms :
- weakly typed (you dont have to declate the type).
- oop 
- versatile langugue (js run on browser/pc/server can run on diffrent platform)

## Varibales 
we can declare varibale by 3 keywords.
- var
- let
- const.

let and const diffrent from var by the scope. 

const is immutable.

>>TODO: ADD COLOUSRE

## Functions

we can use functions in 2 ways :
1. Declared Function :

```javascript
function someFunction(args) {
...
}
```

2. Arrow Function
```javascript
const someFunction = (args) => {...} 
```


why arrow functions matter ? 
we have this code :
```javascript
btn.addEventListener('click',this.addName())
```

Will be executed addName when js compile the code no relate to when press on button.
```javascript
btn.addEventListener('click',this.addName)
```
will fix the problem (we pass reference)
but we have another problem
btn.addEventListener('click',this.addName)

addName() take properties of his class 
when we call it like that this.prop1 , this.prop2 will refer to this of btn and not of the required class
to prevent this we can use bind 
this.addName.bind(this)


using arrow functions 
```javascript
btn.addEventListener('click' , () => this.addName())
```

## Reference VS Primitive 
primitve  are copied value. 

Are stored in the stack not in heap
Type of primitives : 
- String
- Number
- Boolean
- Undefiend
- Null
- Symbol(es6)


>This is happens because we clone the value from the stack

```javascript
let a = 1;
let b = a;
a=88
console.log(a) //88
console.log(b)//1
```

        
> reference ,stored on the heap

Type of reference : 
- Object
- Array
> explore more types


```javascript
let a = {'name':'aviad'}
let b = a;
a.name = 'nahum'
console.log(a) //{'name':'aviad'}
console.log(b) //{'name':'aviad'}
```

if we want to make deep clone we can use it by Object.assign.

But Object.assign clone only primitive and not reference object. 

for that we will have to insert external modules (loadbash , etc...)

## how do we declare object ?

```javascript

const person = {

// field : value
    name : 'aviad',

// functions 
    greet() {
    console.log('hello from' + this.name)
    }
}
```

## Arrays 
array can contain any type of object 
declare array : const exampleArr = [1,2,"hello",{"name":"pinhas"}]
> TODO : is arraylist or linkedlist?

## Spread Operator
copy array :
if we wanna copy array we can use slice , exampleArr.slice();
or use ... : const newArray = [...exampleArr]
this is working with copy an object
const objectOne = {'name' : 'aviad'}
const objectTwo = {...objectOne}
rest operators                          
if we want to allow function give how many argument that she would like
function a(...args){
return args[0]+args[1]
} 

a(1,2)
a(1,3,4,5,5)

Destructring 
take propert quickly from object

const person = {'name' : 'aviad' , age :21}
const name = ({name}) => console.log(name)
name(aviad) // aviad



# Node Basics

## Core Modules : 
- **http**  creating a server , handle http request response 
https - http + ssl 
- **fs** - write/read from fileSystem
- **path** - get path of linux/widnows helper 
- **os** - get relevant info about opertating system.

## Module
If we want take part of code to another file we can use module.
The code can be our code or some online code avilable in npm.

We can import 2 kinds of module
- Global Module - module that we get from npm
- Local Module - module that we are creating

#### Import Module
to import module just use require keyword
```javascript
const fetch = require('fetch') //global module
const cars = require('models/cars') //local module
```

#### Export Module
we can export by 
```javascript
module.exports = someObject;
```
import my module.
```javascript 
const myMoudle = require('./myMoudle.js')
```

Export specific things :

```javascript         
module.exports = {someObject1,someObject2,someObject3};
```

Import speific things : 

```javascript         
const myMoudle = require('./myMoudle.js');
myMoudle.someObject1
```

Short way :
```javascript         
console {someObject1} = require('./myMoudle');
```



if we want to kill node process , process.exit() , shut down the event loop

## HTTP Module
### Request
main properties of object are : 
- req.url  what comes after the host , localhost:3000/test ,  url is test
- req.headers = headers of request
- req.method = GET/POST/PUT/DELETE

### Response
main properties of object are : 
- res.setHeader()
- res.write() - give back to user ,info
- res.end() - end of response 
- res.on() - listen to events 

### How Node transfer data
nodejs stream the data , each time stream part of the data.
Aviad = ['Av','i,'ad'] = 'Aviad' fully parsed , this is byte so this will be like this <Buffer 4d 33 45 ...>
node handle all request in this way

when we want to get data we have to create event addEventListener
```javascript
req.on('data',(chunk) => {...}) //when we get stream of data
```

```javascript
req.on('end',()=> {...}) //when stream has done 
```

Example of recive data from request
 
```javascript
const body = [];
req.on('data', (chunk) => body.push(chunk))
req.on('end', () => {
const parsedBody = Buffer.contant(body).toString(); 
//if it would be a file we would do something else
});
```


## NPM Scripts 
node packge managment 
npm init - this will not delete the code , asks questions about the  project ,this creata package.json file
in json format.
shorcuts to command :
if you want to type npm someCommand and something will happen just open package.json
and add to scripts , script name and command
note that there is list of command that node support.
if you want to name command that node doesnt support add r when you execute the command
npm r my-command (node does not support command)
npm start (node support command)

3rd libaries : how to add ? execute , npm install requiredPackageToDownload
we have development libaries , libaries that we used only to develop not have to be in production and regular libaries
npm install pacakge --save (as production) , npm install --save-dev (development) ,
npm install -g (not in the project but in the machine that you would be able to reuse it in another projects)
in pacakge json we have development dependencies

npm install will install all the required depencdencie that have mentioned in packge.json

## Errors
type of errors : 
- syntax error - forget close braclet and errors like that (linter) 
- runtime errors - not typo , code that breaks when runs (test/      debugger)
- logical errors - you wont get error message , app just not worked (test/debugger)

# EXPRESS JS
what is it express ?
remember that we would like to get request content in node ?
we would have to listen to data , listen to end of the data
and then parse it to required input.
this is nice to learn but in production this is useless.
we want to focus on the buisness logic and not in boilar plate staff.
express wrap node , and let us focus on the bl.
no more all routes in one place diveded by if/else statements , no more listen for buffer.

how to install ? just type npm install --save express

what express is built on ? MIDDLEWEAR 

lets call express instance app
when we use app.use((req,res,next)=>{...}) this will occur on each request
if we would like to call next middleWear we have to speicfy next() else with have to res.write(...),res.end()

app.listen(port) , will create sever. this is save code

replace this 
const app = express()
const server = http.createServer(app)
server.listen(port)

with this 
const app = express()
server.listen(port)

if we want to create custom middlewear for specific route we will add new field 
app.use('/this-my-route',(req,res,next)=>{...})

if we want to redirect the request to another route we will
res.redirect('/route')

if we want to see the content of the body request we are able to use 
req.body => but this if you except to get the content like that you are wrong becuase you have to use body-parser
npm install --save body-parser
add middlewear
app.use(bodyParser.urlencoded(extended:false)) 


routers :
const express = require('express')
const port = 3000;

const app = express()
app.listen(port)
console.log(`server listen to port ${port}`)

app.use('/users',(req,res,next)=>{
res.send('got /users')
})

app.use('/',(req,res,next)=>{
res.send('got /')
})

if we dont want to put all routes in app.js we should use router
to use router we have to create another file
import express
inital router by 
const router = express.Router()
treat the router like express app
router.get((req,res,next)=>{....}) 
at the end export the router
modules.exports = router

in app.js import the router
and make the router as middlewear
app.use(router)

if we want that all the routes in the router will start with common path
we are able to do this thing
app.use('/admin',adminRouter)

if we want to send html file , create new file of html , and use res.sendFile(..pathOfYourFile...)
if we want to link to this file css file we have to use express for that link
we'll open directory named public and put their all the public things , like css ,images ,etc..

and add for express middleWear that the file could acsess this dir by : app.use(express.static(path.join(__dirname, 'public')));

now you can acsess from you html file , like you were in the public dir 
example of take css file <link rel="stylesheet" href="/css/main.css">

Template Enginges
put dynamic content on html
we have the html file with place holder
we have html engine that replace the place holder with real content 
we have common template engines like : EJS , Pug , Handlebars

app.set('name','value') // set global configuration value on express app
we can read this via app.get('key')

activate pug as template engine : 
app.set('view engine', 'pug');

pug wil compile to regular html

MVC (Model Views Controlles)
Models 
Represnt your data in code
work with your data (save,fetch,etc..)
Views
What are users sees
Decoupled from your application
Controllers
Connectiong your Models and you Views
Contains the "in-between" logic


# Dynamic Routes

end point :
example : localhost:3030/getUserInfo/aviad
here we have a get request that we get user name and return info about person
to create this in express just use
app.get("/getUserInfo/:userName")

/: = end point

the order that we present the routes is super important 
for example 

app.get("/getUserInfo/delete")
app.get("/getUserInfo/:userName")

in this case delete will never run , the second route will override him each time
so we should put the route with the endpoint in the end

acsses end point param

/product/:id
const id = req.param.id

query param : 
you are better be familliar with this
localhost:3000?param1=value1&param2=value2
this is optional data
value always be string in express

acsses querm param

/product&edit=true

const edit = req.query.edit

# SQL
quikcer than store in file,
we dont have to search whole file to find tiny information

sql vs no sql

sql relates on tables. 
each row called record.
you can relate between diffrent tables
we have data schema (define how table will look like) each record has follow schema
Data Relations (One-to-One , One-to-Many , Many-to-Many)
sql means stractred query langugue (looks like english , SELECT * FROM users WHERE age > 28)
vertical scaling. (scaling by adding more cpu,memory)
limitations for lots read&write queires per secons

no-sql 
you have db that store collections(=equivliant to table), 
each collection store document(equivliant to record)
no sql doesnt have strict schema , we usally duplicates data.
no data relations.
horizontal scaling. (scaling by adding servers)
great performance for mass read & write requests

# MY SQL

install by 
```javascript
npm install --save mysql2
```

we have to options 2 talk with db. 
- open connection each time we have to talk with the db.
- open connection and use it when neccisery.


## example for integrate with mysql server :

1. run command ```npm install --save mysql2```
2. create a config object that contains 
(host ,user,password ,port(deafult=3306) 
3. create new moudle named database
create a pool , and give the pool the config. 
```javascript
const config = {host:'', ....}
const sqlPool = mysql.createPool(config)
module.exports = sqlPool;
```

whenever you want to integrate with db,
import sqlPool
and by sqlPool.execute('run you sql command').then().catch()
this is supporting promise  

```javascript
static fetchAll() {
return db.execute('SELECT * FROM products')
}
```

```javascript
exports.getProducts = (req, res, next) => {
Product.fetchAll().then( ([products]) => {
res.render('shop/product-list', {
prods: products,
pageTitle: 'All Products',
path: '/products'
});
});
};
```

## sequlize
let you foucus on your model logic.

**genreate behind the scens the sql query**

sequlize need the mysql2 that we have installed
to start with sequlize we have to inital it 
so we should create new moudle named database (or whatever you want)

we are going to inital the connection via sequlize


   
1. Inital Sequlize.
 ```javascript
const sequelize = new Sequelize('db-name', 'username', 'password', {
dialect: 'mysql',
host: 'localhost',
port: 3308
});
```
2. then in app we are going to inital all the modles 
(behind the scens what runs is sql query for create table if not exists for each model)
for exmaple :
```sql
CREATE TABLE IF NOT EXISTS `products` (`id` INTEGER NOT NULL auto_increment ,
`title` VARCHAR(255), `price` DOUBLE PRECISION NOT NULL, `imageUrl` VARCHAR(255) NOT NULL, 
`description` VARCHAR(255) NOT NULL, `createdAt` DATETIME NOT NULL,
`updatedAt` DATETIME NOT NULL, PRIMARY KEY (`id`)) ENGINE=InnoDB;
```
in this example if the db connection not succssed the server is not up.
we inital after db sync has done.   
```javascript
const port = 3000;
sequlize.sync().then(result => {
console.log('connect to db')
app.listen(port,()=>console.log(`server listen to port ${port}`));
})
```

### Creating and using models :
so lets assume that we would like to represnt new model.


Our model is Car , that will contain , pk(id), model(String), manufactringYear(int).

so we will create in package models new moudle named cars.

```javascript
const Sequelize = require('sequelize')
const sequelize = require('../util/database')

const Cars = sequelize.define('product', {
id: {
type: Sequelize.INTEGER,
autoIncrement: true,
allowNull: false,
primaryKey: true
},
manufactringYear: {
type: Sequelize.INTEGER,
allowNull: false
},
model: {
type: Sequelize.STRING,
allowNull: false
}
});

module.exports = Cars
```

who will need to use this model , will use functions of sequlize
for example fetch all cars will be
```javascript
Cars.findAll()
```
behind the scens sequlize will run 
```sql
SELECT * FROM cars
```

### find
 we have 2 ways : 
* Cars.findByPk()
* Cars.findAll({where: {id: someId }}) 

### insert 
>>complete
### updated
>>complete
### delete
>>complete







