## Pass parameter while executing node program

So we if would like to pass to some script parameter , we can do it while we executing script.

for example :

`hello-world.js`

```javascript
console.log('hello world !')
```



if we would like to , say hello world to special person , and we want it to be changed.

we can use it 

```javascript
console.log(`hello world ${process.argv[2]}`)
```

by running `hello-world.js Aviad`

this will print `hello world Aviad`

`process.argv` is global variable with type of array  than contains the parameter that we gave to scripts.

this array always contains 2 elements

`'node', '/path/to/your/baby-steps.js'` 

- first is the command that we used to run the script , in this case is `node`.

- second is the path of the running script.

---

## Call back & Promises & async/await

