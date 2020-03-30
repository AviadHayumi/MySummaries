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



# BRIEF OF JS 
pardigms :
- weakly typed (you dont have to declate the type).
- oop 
- versatile langugue (js run on browser/pc/server can run on diffrent platform)

## Variables 
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
NPM is a shortcut of **N**ode **P**ackage **M**anagement

```npm init``` - will create a package.json file that will containts :

-  details on project , meta data.
-  information running scripts. 
-  required modules.

This command asks questions about the  project ,this creata package.json file
in json format.

For skipping questions run ```npm init -y```

shortcuts to command :
if you want to type npm some-command and something will happen.

1. just open package.json
2. and add to scripts , script name and command
   note that there is list of command that node support.
3. if you want to name command that node does not support add run when you execute the command
   ```npm run my-command```



3rd libaries : how to add ? 

execute ,``` npm install your-module```

we have development libraries , libaries that we used only to develop not have to be in production and regular libaries
npm install pacakge --save (as production) , npm install --save-dev (development) ,
npm install -g (not in the project but in the machine that you would be able to reuse it in another projects)
in pacakge json we have development dependencies

npm install will install all the required depencdencie that have mentioned in packge.json

```json
{
  "name": "server",
  "version": "1.0.0",
  "description": "some description",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start" : "nodemon index.js"
  },
  "keywords": [],
  "author": "Aviad",
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.1",
  }
}

```



## Errors

type of errors : 
- syntax error - forget close braclet and errors like that (linter) 
- runtime errors - not typo , code that breaks when runs (test/      debugger)
- logical errors - you wont get error message , app just not worked (test/debugger)

# EXPRESS JS
### what is it express ?
remember that we would like to get request content in node ?
we would have to listen to data , listen to end of the data
and then parse it to required input.

this is nice to learn but in production this is useless.
we want to focus on the business logic and not in boiler plate staff.

express wrap node , and let us focus on the bl.
no more all routes in one place divided by if/else statements , no more listen for buffer.

how to install ? just type ```npm i --save express```



### what express is built on ?

 MIDDLEWEAR 

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

## SQL VS NO.SQL

### SQL

- relates on tables, each row called record.

- you can relate between different tables

- we have data schema (define how table will look like) each record has follow schema
  Data Relations (One-to-One , One-to-Many , Many-to-Many)

- SQL means Structured Query Language (looks like English , ```SELECT * FROM users WHERE age > 28)```

- vertical scaling. (scaling by adding more cpu,memory)

- limitations for lots read&write queries per seconds

  

### NO.SQL

- you have db that store collections(=equivalent to table), 
- each collection store document(equivalent to record)
- doesn't have strict schema , we usually duplicates data.
- no data relations.
- horizontal scaling. (scaling by adding servers)
- great performance for mass read & write requests

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

whenever you want to integrate with db.
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
	Product.fetchAll().then(([products]) => {
		res.render('shop/product-list', {
			prods: products,
			pageTitle: 'All Products',
			path: '/products'
		});
	});
};
```

## Sequlize
Sequlize let you focus on your model logic.

**genreate behind the scens the sql query.**

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
for example :
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
behind the scenes sequlize will run 
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





# MongoDB

MongoDB is basically NO.SQL database.

His name was taken from the word Humongous because one of his main target is to store and work with a lot of data , with nice scaling.

Mongo use json to store documents , actually mongo db store the json in bson format.



when using nodejs and node , best practice is to create user in mongo with read&write prefrences this user will be used in nodejs application.



```npm i --save mongodb``` 



# Mongoos

mongoos is odm for mongo which similar to orm.

object document model.

we works with schema and models



npm i --save mongoose

mongoose manage the connection to the database.

```javascript
(async () => {
try {
	const result = await mongoose.connect('dburl')
	console.log('mongodb connected')
	app.listen(port)
}
catch(err){
    console.log(err)
}
})()
```



### working with models

For working with model , we have to define a schema :

```javascript
const {Schema} = require('mongoose')

const productSchema = new Schema({
    title : {
        type : String,
        required : true
    },
    price : {
        type : Number,
        required : true
    },
    description : {
        type : String,
        required : true
    },
    imageUrl : {
        type : String,
        required : true
    }
});

```

No worried we will be able to change the schema on the flight.

required means we have to have this field.

type meaning is what is the type of field.



save will save if not exists else update.



populate is like join

```javascript
const products = await Product.find().populate('userId')
```

will give us products not with userId field but with the entire user that is related to this.



select is like project

```javascript
const products = Product.find().select("title price -Id")
```

will be converted to this mongo query 

```javascript
db.products.find().project({title : 1, price : 1, _id :0})
```

will not present id. 

to make references in schema the field that is referring to another collection will contain

```javascript
   type : Schema.Types.ObjectId,
                ref : 'Product',
                required : true
```

ref will represent reference to another model.

### Add new functionality to our model





# Sessions & Cookies

Let's assume we want to create login page for our website.

As first step we expose some element in our nav-ar as condition if user is logged in.



We can save on request data , 

So what we can do is save on request information of isLoggedIn

```javascript
req.isLoggedIn = true
```

**but** our big problem is that when we send response the information that we've saved on the request is not available for us.



What we can do in these case is using cookie.

if you would like to save some data in the browser you can do it by  sending header with data.

```javascript
res.setHeader('Set-Cookie','loggedIn=true')
```



now after this change , the browser default send the cookie data no matter what is it the request.

For access the cookie from our server we can do it by  

```javascript
req.get('Coockie')
```

we will get text of all of our cookies data

for example ``` isLoggedIn=true;isBlackTheme=true``` 



we've a big problem with this approach , the user can easily change the cookie.

to access the cookie the user can open devTools and change the cookie to whatever he would like to.

for example if we have ```isVipUser=false;``` the user can easily make this to true ```isVipUser=true```



the cookie don't need to be related to your page ,

google can track our use of website by cookies.



## Configure Cookie

1. put ttl to the cookie. 	

```javascript
res.setHeader('Set-Cookie','loggedIn-true; Max-Age=10' )
```

2. Domain , where to send the cookies

```javascript
res.setHeader('Set-Cookie','loggedIn-true; Domain=url' )
```

3. serve cookies only if there is ssl layer (https)

```javascript
res.setHeader('Set-Cookie','loggedIn-true; Secure' )
```

4. HttpOnly - can't access cookie from client side javascript (rescue from attack scripting) 

```javascript
res.setHeader('Set-Cookie','loggedIn-true; HttpOnly' )
```



## Sessions

instead of save the information of user is logged In on the client side , which is bad practice save it in the back-end. 

this called session , a construct between user and server.

Session is saved in the database.

The client save in the cookie the id of the session.

the id that we store is usually hashed with an algorithm.



### Session & Express

first install this package

```javascript
npm i --save express-session 
```

```javascript
const session = require('express-session')
```

 ```javascript
app.use(session({
  secret: 'my secret', 
  resave: false , 
  saveUninitialized : false,
  cookie : {
      Max-age:10,
      ...
  }
}));

/*
secret - sign the hash
resave - sion will not be saved on every respone that change but only in change
saveUninitialized - ensure that no session gets saved for a request where it doesn't need to be saved because nothing was changed,
coockie - configure the cookie properties
*/
 ```

we can do this 

```javascript
req.session.isLoggedIn = true;
```

what the client will see is this 

```javascript
connect.sid : s%3AzgQll73ywznmerr-7Xt58yw-Aa4CHM2i.Te69dgmZv0465s0EG8Vv3f4hXHoIupttWCQEAB0JUnI
```

instead of 

```javascript
isLoggedIn = true
```

now we can be sure that the client cannot play with the cookie property.

in the server side we don't have to decode this hashed value , this happens automatically by the middle-ware 

```javascript
console.log(req.session.isLoggedIn)
```



actually as we can see , the data is hashed and saved in the client side , only the server-side can decode the hashed data and get the value from there.

with this way we can protect the data.



## Integrate MongoDB with sessions

install package

```javascript
sudo npm i --save connect-mongodb-session
```

```javascript
const MongoDBStore = require('connect-mongodb-session')(session);
```

```javascript
const store = new MongoDBStore({
  uri:mongoUri,
  collection: 'sessions'
});
```

```javascript
app.use(session({
  secret: 'my secret', 
  resave: false , 
  saveUninitialized : false,  
  store : store
}));
```

now we can see that the session is stored on our database.

```json
{
	"_id" : "7VyRgRK1YlyL7ra6ujowcMPoTx_Avxid",
	"expires" : ISODate("2020-03-21T22:19:53.234Z"),
	"session" : {
		"cookie" : {
			"originalMaxAge" : null,
			"expires" : null,
			"secure" : null,
			"httpOnly" : true,
			"domain" : null,
			"path" : "/",
			"sameSite" : null
		},
		"isLoggedIn" : true
	}
}
```



we can delete session from browser and from database by 

```javascript
 req.session.destroy(()=>{
     //action to prefrom when session is destoryed , for example : 
     res.redirect('/');
 });
```



# Authentication

## Protect Routes

so we would like to add authentication to our app.

we're going to save on the session :

- the user information
- is loggedIn

each time the user try to access route that required authentication , we'll check if the user loggedIn from session.

if not loggedIn  we'll redirect him to loggin.



```javascript
module.exports = (req,res,next) => {
    if(!req.session.isLoggedIn){
        return  res.redirect('/');
    }
    next();
}
```



now we are going to put this middleware on required routes

```javascript
const isAuth = require('../middlewares/is-auth');
router.post('/cart-delete-item', isAuth , shopController.postCartDeleteProduct);

```



put attention that we can put in post/get/delete/use/put/etc... as much function of req,res,next as we want.

the middleWares will be called from left to right.



## SignUp

so we've protected our routes , now let's get into create user.

user will contain :

- username
- password

we don't want to save user password as is in datbase , so what we are going to do is hash it with bycrpt.

```javascript
const bcrypt = require('bcryptjs');
exports.postSignup = async(req,res,next) => {
  console.log(req.body)
  const email = req.body.email;
  const password = req.body.password;
  const confirmPassword = req.body.confirmPassword;

  try{
    const isUserExists = await User.findOne({email : email});
    if(isUserExists){
      return res.redirect('/signup');
    }
    
    const hashedPassword = await bcrypt.hash(password, 12); //generate hash password

    const user = new User({
      email: email,
      password : hashedPassword,
      cart : {items : [] }
    })

    const result = await user.save();

    if(result){
       res.redirect('/login');
    }

  }catch(err){
    console.log(err);
  }


  next();
}
```



## Login

```javascript
exports.postLogin = async(req,res,next) => {
  try{
    const email = req.body.email;
    const password = req.body.password;

    const user = await User.findOne({email : email});
    if(!user){
      return  res.redirect('/login');
    }

    const isPasswordEquals = await bcrypt.compare(password,user.password);

    if(!isPasswordEquals){
      return  res.redirect('/login');
    }

    req.session.isLoggedIn = true;
    req.session.user = user;
    req.session.save(()=>{
      res.redirect('/');
    })

  }catch(err){
    console.log(err);
  }
}
```



## Cross Site Request Forgery Attack

CSRF is common attack that the hacker using the cookies&sessions to do whatever he want.

he take to cookie , and sending to server by api what he want like transfer money from place a to b.

To prevent this attack what we can do is generate hash in client form , and check when the request occur that the hash from form is valid , we can assure that the request will be only from ui website.



```javascript
npm i --save csurf
```



```javascript
const csrf = require('csurf');
app.use(csrfProtection);
```



when we render the views , we'll pass the token

```javascript
exports.getIndex = (req, res, next) => {
  Product.find()
    .then(products => {
      res.render('shop/index', {
        prods: products,
        pageTitle: 'Shop',
        path: '/',
        isAuthenticated: req.session.isLoggedIn,
        csrfToken : req.csrfToken()
      });
    })
    .catch(err => {
      console.log(err);
    });
};
```

```ejs
 <form action="/logout" method="post">
                        <input type="hidden" name="_csrf" value="<%= csrfToken %>"
                        <button type="submit">Logout</button>
 </form>
```

name=```_csrf``` is to identify from where input the csrf value token exists.



instead of inject in in each time rendering views

```javascript
 isAuthenticated: req.session.isLoggedIn,
  csrfToken : req.csrfToken()
```

what we can do is inject in automatically.

 ```javascript
app.use( (req,res,next) => {
   res.locals.isAuthenticated = req.session.isLoggedIn,
   res.locals.csrfToken = req.csrfToken() 
})
 ```



# Express Sending Emails

we are going to use third party service to send emails.

we are going to use sendGrid.

```javascript
sudo npm i --save nodemailer nodemailer-sendgrid-transport
```

```javascript
const nodemailer = require('nodemailer');
const sendgridTransport = require('nodemailer-sendgrid-transport');

const transporter = nodemailer.createTransport(sendgridTransport({
  auth : {
    api_key : 'your-key'
  }
}));

await transporter.sendMail({
    to:email,
    from: 'shop@node-complete.com',
    subject: 'sign up succeeded',
    html : "<h1> you successfully signed up! </h1>"
})

```



when we want to scale we way not sending emails from here , 

we'll use message queue or script that will run in schedule find new users send them email.



# Validation

validate that the data we work with is valid and in the format we want.

for example validate email is valid.



when we validate data on client side this is not secure because requests still can be sent to the server.

we'll use express-validator

```javascript
npm i --save express-validator
```

we'll not validate in get but only in post/put.



we'll going to validate sign up form.

route : 

```javascript
const {check} = require('express-validator/check');
router.post('/signup',check('email').isEmail(),authController.postSignup);
```

controller :

```javascript
const {validationResult} = require('express-validator/check')

exports.postSignup = async(req,res,next) => {

  const errors = validationResult(req);
  if(!errors.isEmpty()){
    return res.status(422).render('auth/signup',{
      path: '/signup',
      pageTitle: 'SignUp',
      isAuthenticated: false,
      errorMessage : errors.array()[0].msg
    })
  }

  const email = req.body.email;
  const password = req.body.password;
  const confirmPassword = req.body.confirmPassword;
  ...signupLogic
}
```

create custom validator. 

```javascript
check('email')
	.custom((value , {req})=>{
    	return value=="aviad"
})
```



to validate certain fields we can put any check as element in array

[check(..),check(...)]



# Error Handling

error in js is an object.

when error occurs we have different ways to handle it :

- if the code is synchronized put it in try catch and handle exception,
- if code is async use then() catch() or try catch (with async await syntax)
- use middle ware to handle this exceptions 

we can redirect user to error page , tell the user what was wrong in input 



if you want to create middle ware for error just add another argument 

```javascript
app.use((error,req,res,next)=> {
    ....
})
```

we can specify error middleware for specific path. 

```javascript
app.use('/path', (error,req,res,next)=> {
    ....
})
```



if error will occurd in sync code , this will go as default to error middleware.

if error occur in async code , we have to ```next(err)```

 

# Files

we are going to focus on two things : 

- upload file.
- serve file from server.



## Upload File

to upload file we have to support it by our parser , till now our parser parse text , now we need to tell him parse files as well.

also we need to change our form that to support upload files in their request 

```html
<form ... enctype="multipart/form-data" >
  ...  
</form>
```

```enctype="multipart/form-data"``` means form will contain text &  **files**



body parser of files is multer

```javascript
npm i --save multer
```



multer as default handle the files as buffer and take metadata like : 

```json
{
    fieldname : 'image',
    originalname : 'boat.png',
    encoding : '7bit',
    mimtype : 'image/png',
    buffer : <Buffer ...>,
    size :5000
}
```



we can configure multer save the image in specific place in file-system :

```javascript
app.use(multer({
  dest : 'images', //save the image in this path 
}).single('image'));
```



 we can filter the type of files we get by

```javascript

const fileStorage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, 'images');
  },
  filename: (req, file, cb) => {
    cb(null, new Date().toISOString() + '-' + file.originalname);
  }
});
const fileFilter = (req, file, cb) => {
  if (
    file.mimetype === 'image/png' ||
    file.mimetype === 'image/jpg' ||
    file.mimetype === 'image/jpeg'
  ) {
    cb(null, true);
  } else {
    cb(null, false);
  }
};


app.use(
  multer({ storage: fileStorage, fileFilter: fileFilter }).single('image')
);

```



## Serve File

we are going to treat the app as statefull therefore save the images on the project.

make images to public.

```javascript
app.use(express.static('/images',path.join(__dirname, 'images')));
```

now we can access to this dir from anywhere.

### Send files

we can send to the client file , the client will download it.

for example if we have pdf we can send it to client.



```javascript
exports.getPdfFIle = (req,res,next) => {
    fs.readFile(path,(err,data)=>{
        if(err){
            return next(err);
        }
        res.setHeader('Content-Type','application/pdf');
        res.send(data);
    })
}
```



**Don't use it on big files!**

node will read entire file to the memory and then serve it.

bigger files cause memory leaks and a lot of time waiting.

in big file we'll steaming the data.



```javascript
const file = fs.createReadStream(path);
res.setHeader('Content-Type','application/pdf');
res.pipe(file)
```



## 	Create pdf 

```javascript
npm i --save pdfkit

const PDFDocument = require('pdfkit');

const pdfDoc = new PDFDocument();
res.setHeader('Content-Type', 'application/pdf');
res.setHeader(
    'Content-Disposition',
    'inline; filename="' + invoiceName + '"'
);
pdfDoc.pipe(fs.createWriteStream(invoicePath));
pdfDoc.pipe(res);

pdfDoc.fontSize(26).text('Invoice', {
    underline: true
});
pdfDoc.text('-----------------------');
let totalPrice = 0;
order.products.forEach(prod => {
    totalPrice += prod.quantity * prod.product.price;
    pdfDoc
        .fontSize(14)
        .text(
        prod.product.title +
        ' - ' +
        prod.quantity +
        ' x ' +
        '$' +
        prod.product.price
    );
});
pdfDoc.text('---');
pdfDoc.fontSize(20).text('Total Price: $' + totalPrice);

pdfDoc.end();
```



# Pagination

split data across multiple pages instead of returning all the data.

just use skip and limit to manipulate the data.

from front end ask for pages , in backed do

const limit = limitPerPage;

const skip = (page-1) * limitPerPage;



we can send csrf as header

'csrf-token' : '...'

csurf will check there automaticlly



# Payments

when we need to add functionality of payment to our service , we need to add : 

- Collection Payment Method
- Verify Payment Method
- Charge Payment Method
- Manage Payments
- Do our logic

usually all the 4 steps implemented by 3rd library.



We are going to use Stripe as 3rd library

How does it work ?

Client will collect credit card data and send to stripe 

stripe return token

in our code we will create charge with help of stripe (we'll send token and charge)

we'll get a response when this is charged.

# REST API

imagine that you have an some types of clients for your server.

you have an android app , ios app , single page application (react) , and microservice that use it.

if we will return only html as result this will be problemtic for our clients.



Rest Api = Representational State Transfer

we give who call the api only the data , usually as json and we give him the change to choose what to do and how to represent the data.



api endpoint is combination of : 

- http method

- http path

  for example : ```GET http://localhost/hello-world``` 

HTTP METHODS :

- Get - get a resource 
- Post - Sends data to the server for creating a new entity. Often used when uploading a file or submitting a web form
- Put - Similiar to POST, but used to replace an existing entity.
- Patch - Similar to PUT, but used to update only certain fields within an existing entity
- **DELETE**: Removes data from the server.
- **TRACE**: Provides a way to test what server receives. It simply returns what was sent.



Rest Principles :

- Clearly defined api endpoints , the user know what he gives what he gets back, be clearly , documents what needed , good naming.

- Stateless Interactions - server and client don't save any connection history , like isLoggedIn on the session , every request is handled separately.

- Cachable - chancing headers , allow cache result and for how much time.

  etc...

201 success created.



CORS : 

OPTIONS : means which type of request you allow to your server , before fetch will happen , options request will be sent from the browser to check that this is valid.



allow-some-websites access your server ```Access-Control-Allow-Origin : * ```

allow some website access with content type ```Access-Control-Allow-Headers : Content-Type``` 



allow all clients access to your api  :

```javascript
app.use((req,res,next)=>{
    res.setHeader('Access-Control-Allow-Origin', '*')
    res.setHeader('Access-Control-Allow-Methods', 'GET,POST')
    res.setHeader('Access-Control-Allow-Headers', 'Content-Type,Authorization')
	next();
})
```





Put and Patch speical because they have paramters and request body.



## Authentication Rest Api

client authenticate to server , server send token that called jwt (json web token).

from there the client need to store the token and send it as additional data when 

he want to fetch some data from the server.



```javascript
npm i --save jsonwebtoken
```



Login user and return json web token.

```javascript
const jwt = require('jsonwebtoken');

exports.login = async (req,res,next) => {
    try {
        const email = req.body.email;
        const password = req.body.password;
        
        const user = await User.findOne({email : email});
        if(!user){
            const error = new Error('User with this email not defiend.');
            error.statusCode = 401;
            throw error;
        }

        const isPasswordEquals = await bycrpt.compare(password,user.password);
        if(!isPasswordEquals){
            const error = new Error('Password not match.');
            error.statusCode = 404;
            throw error;
        }

        const token = jwt.sign({
            email : user.email,
            userId : user._id.toString() 
        }, 'somesupersecretsecret',
        {expiresIn : '1h'})

        res.status(200).json({message : 'password match', jwt : token});

    }catch(err){
        next(err);
    }
}

```



Validating jwt : 

usually in jwt we put the token in a header.

Header :```Authorization : 'your token'```

(you need to enable this header in cors)

the convention is to put the word Barer before the jwt

```json
Authorization : Barer  eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImF2aWExMzIzMjJkQGdtYWlsLmNvbSIsInVzZXJJZCI6IjVlN2VhYjhhOTIwM2UwNjU2ZjhhMDBmOSIsImlhdCI6MTU4NTM2MTIxMywiZXhwIjoxNTg1MzY0ODEzfQ.dmYwH0QRdyy2XxVfBeg8UA9iDWXXq1NTUI81POeglyY
```

```javascript
const jwt = require('jsonwebtoken');

module.exports = (req,res,next) => {
    const header = req.get('Authorization');
    if(!header){
        const err = new Error('Not authenitcated')
        err.statusCode = 401;
        throw err;    
    }
    const token = header.split(' ')[1];
    let decodedToken;
    try {
        decodedToken = jwt.verify(token , 'somesupersecretsecret');
    }catch(err){
        err.statusCode = 500;
        throw err;
    }

    if(!decodedToken){
        const err = new Error('Not authenitcated')
        err.statusCode = 401;
        throw err;
    }

    req.userId = decodedToken.userId;
    next();
};
```





# Web Sockets



Socket.io is built upon a websocket , and manage by engine.io things like keep alive , etc..

Therefore we can't just connect from socket-io-client to regular websocket , and vice versa.

to manage to who will message sent use namespaces.

```javascript
npm i --save socket.io
```

```app.js```

```javascript

(async ()=> {
	const server = app.listen(8080);
	const io = require('./socket.js')(server);
    
})()


```

```socket.js```

```javascript
let io;

module.exports = {
    init: httpServer => {
        io = require('socket.io')(httpServer);
        return io;
    }
    getIO : () => {
        if(!io){
            throw new Error('Socket.io not initlized');
        }
        return io;
    }
}
```

```feed.js```

```javascript
const io = require('../socket');

exports.createPost = (req,res,next) => {
    ...
    //emit sends to all user
    // brodacst send to all users except use doing operation
    io.getIO().emit('posts' , {action : 'create', post:post})
    ...
}
```



client side

```javascript
npm i --save socket.io-client
```

```javascript
const socket = openSocket('http://localhost:8080');

socket.on('post', data => {
    if(data.action == 'create') {
        this.addPosts(data.post);
    }
})
```



# GraphQL

**Rest On Steroids**



## What is it GraphQL ? 

Let's assume that

```GET /POST``` 

return

```json
{
    id : '1',
    title : 'First Post',
    content : 'Great Book',
    creator : 'Aviad'
}
```



we if we only need the title ?

in another request we need title and content.

1. create end-point to each require is difficult to maintain.

2. we can use query params but the api will be hard to understand.

3. use GraphQL , the client query whatever he wants.



in GraphQL we only send ```POST``` request that contains in the body the data that should be returend.

for example :

``` POST /POST```

```json
{
    query { // Operation type 
    	user {
    		name
    		age
		}
	}
}
```



we 3 Operation types :

- query - get data from server , filter whatever you want

   equivalent to ```GET```

- mutation - editing , deleting , inserting , subscriptions data.
   equivalent to ```POST , PUT , PATCH , DELETE```
- subscription - subscribe data to get real time data.



type definition will be equivalent to routes

resolver equivalent controller.



