# GoLang

This is going to be mixed summaries of `Coursera course of golang` & the book 

`The Go Programming Language Book` .



# Course

 ## Getting Started With Go 

go doc https://golang.org/doc/#learning



### why should i learn go ?

1. Code runs fast.
2. Garbage collection (most language that runs so fast don't have gc)
3. Simpler objects (simplify oop)
4. concurrency is efficient.



Code runs fast & Garbage collection

Software Translation

There are 3 types of software translation :

1. Machine language , CPU instructions represented in binary,
2. Assembly language , CPU instructions but easier to read than bytes.
3. High level language (c , c++ , Java , Python , Go ,etc..) much easier to use.

go is high level language 



when we want to run high level we need to translate the code to machine code that the processor will understand it.



Complication - translate instructions once before running the code.

Interpretation - translate instructions while code is executed requires an interpreter to every time we run this.



compiled is fast , in some language there are manual memory language like c.

interpreter make coding easier , handle for you thing that you don't want like type deceleration , manage memory management automatically.



Go have garage collection.



OOP

in most common languges we have :

class defiens data and functions.

object are instance of class.



in go we have

`structs` with associated methods and data. like class.

in go we don't have inheritance , constructors , generics.

instead of using inheritance we use composition.





concurrency

concurrency comes from the need for speed.

Moore's Law used to help performance 

more transistors used to lead higher clock frequencies.

power temperature constrains 

we can't get crazy with limit clock frequencies because he will melt.



what can we do to add speed ?

- parallelism - increase number of cores , you can do multiple tasks at the same time on different cores.

  ​						difficult : when task start/stop ?

  ​						what if one task needs data from another task ? 
  ​						do tasks conflict in memory ?

- Concurrent - management multiple tasks at the same time (they are alive in the same time , they could run in single core , then each time another thread will be executed for couple ms)

  concurrent allows parallelism , management task execution , communication tasks (share memory) , sync between tasks(this task will run after this task).



Gorounites represent concurrent tasks

channels are used to communicate between tasks

select enables task sync.



workspace

in go we will contains: 



src - all source files



pkg - all libraries (group of related source files , each package can be imported by other packages , enable software reuse)

import package 

```go
import (
	"annpkg"
    "billpkg"
)
```

there will be always in a project package that have to be named `main` 

when we build `main` this create executable.

in main is where code start.

```go
package main
import (
	"fmt"
)
func main(
    fmt.Printf("hello" , "world\n")
)
```



bin - contains executables



GOPATH - defiened where go dir exists.

GOROOT ?



GoTool

when we doing import the GoTool find the spefic package by GOROOT GOPATH.

when you download go you get GoTool that has bunch of commands.

`go build` compiles the program , and create an executable for the main package.

`go doc` prints doc for a package.

`go fmt` formats source code files.

`go get` downloads packages and installs them.

`go list` list all packages

`go run` compiles .go files and runs the executable.

`go test` runs tests using files ending in `_test.go`



variables

naming - variables must start with a letter ,  can have any letters digits and underscores , case sensitive , they are saved keyword (if ,case ,etc...)

 each variables must have name and type.

```go
var x int // specify varible name x type int
var x,y // declare two varibles
```



we can use aliases

```go
type Celsius float64
type IDnum int

var temperatue Celsius // this will be float64
var pid IDnum //this will be int
```



initialize varible

```go
var x int = 100
var x = 100 //this will check type and put this integer

var x int
x = 100

x := 100 //equivilant to var x = 100 , only can do this inside a function
```

if you don't initialize variable will get default value

string = "" , int = 0



## Data Types

Pointers

pointer address to data in memory

functions and variables are located in memory.

there are two operators two pointer

`&` operator returns address of a variable/function

`*` operator return data at an address.

```go
var x int = 1
vat y int
var ip *int // ip is a pointer to int

ip = &x // ip now points to x
y = *ip // y is now 1 , this is no going to point on x , i'll change x y won't change
```

alternative to create a variable with `new()` 

`new()` create a variable and returns a pointer to the variable

```go
prt := new(int)
*prt = 3
```



variable scope

where variable can be accessed 

```go
var x = 4

func f(){
    fmt.Printf("%d",x)
}

func g(){
    fmt.Printf("%d",x)
}
```

both function f and g will be able to access to x.

if not find var will go to bigger block and check there.



```go
var x = 4

func f(){
    var x = 2
    fmt.Printf("%d",x) //output 2
}

func g(){
    fmt.Printf("%d",x) //ouput 4
}
```



Deallocating Memory

free memory space to make this available.

functions variable will be saved on stack and they deallocate when functions complete.

global scope variables will be saved on the heap. you have explicitly deallocate it , or let garbage collector do his job

in go Compiler defines where to put variable key or stack.

```go
func foo() *int {
    x := 1
    return &x
}

func main(){
    var y *int
    y = foo()
    fmt.Printf("%d",*y)
}
```

we can notice here that x not really been removed.



Garbage Collection

when variable not in use , there are not pointers to it , he will remove it.



Comments 

```go
//single line comment

var x int //Another comment


/*
BLOCK COMMENT
multi 
line 
comment
*/
```



Printing

```go
fmt.Printf() // format and print string

name := 'Aviad'
fmt.Printf("hi %s", name) // hi Aviad
```



Integers

we have diffrent lenghts of integers

- int8
- int16 (0-64K)
- int32
- int64
- uint8
- uint16
- uint32
- uint64

unit is unsigned integer he can be bigger.

usually we'll specify int and the compiler target required int.

the more you use the bigger the number can be.



Type Conversions

we are converting with `T()`

```go
var x int32 = 1
var y int64 = 2 
x = y // throw an error

x = int32(y) // convert to int32

```



Floating points

- float32 (6 digits of precision)
- float64 (15 digits of precision)
- complex128 (complex(2,3) first real , second imaginary)



Strings

defaults in go is utf8

strings are cannot being change , they read only.

you can create new string.

string literal `x := "Hi there"`



strings package

you can search , compare ,contains,hasPrefix (true if begins), Index(s,substr)

manimuplate : Replace(s,old,new,n) , ToLower , ToUpper , TrimSpace(s)

Strconv package - convert strings

Atoi(s) - convert string to int

Itoa(s) - converts int to string

FormatFloat(f)

ParseFLoat(s,bitSIze)





const

```go
const x = 1.3 //asign one const
const ( //asign many consts
	y = 4
    z = "Hi"
)
```

```go
type Grades int

const (
	A Grades = iota
    B
    C
    D
    F
)

/*
	A = 1
	B = 2
	C = 3
	D = 4
	F = 5
*/
```



Control Structures

```go
if (condition){
    
}

for i:=0; i<10; i++ {
    
}

//like while
isPlaying := true
for isPlaying {
    i++
}

for {
    //infite loop
}


switch x {
	case 1:
    	fmt.Printf("case1")
    case 1:
    	fmt.Printf("case2")
    default:
    	fmt.Printf("default")
}

switch {
	case x > 1 :
    	fmt.Printf("x>1")
	case x < -1 :
    	fmt.Printf("x<-1")
	case default :
    	fmt.Printf("default")
}
```

we have `break` , `continue` like others languages



scan - reads user input

```go
var appleNum int

fmt.Printf("Number of apples ?")

num,err := fmt.Scan(&appleNum)

if(err!=nil){
    fmt.Printf(appleNum)
}
```



Composite Data Types



Arrays

fixes length series of a chosen elements

elements initialized to default value

```go
func main() {

	var x [5]int
	x[0] = 2

	for i:=0; i< len(x); i++ {
		fmt.Println(x[i])
	}
}

/* output
2
0
0
0
0
*/
```



array literal

```go
var x [5]int = [5]{1,2,3,4,5}
```



expressed size of an array literal 

```go
x := [...]int{1,2,3,4}
```



iterate array 

```go
x := [...]int{1,2,3,4}

for index,value range x {
    fmt.Printf("index : %d , value : %d" , index,value)
}
```



Slices

array with flexible size



slice has three properties :

- Pointer - pointer to start array.
- Length - number of the elements in the slice.
- Capacity - the maximum number of elements.



slice not copy array but points to his elements

```go
arr := [...]string{"a","b","c","d","e","f","g"}
s1 := arr[1:3] // b c 
s2 := arr[2:5] // c d e

// if i would change arr[3] c , this will be changed in two slices and array

fmt.Printf("lenght : %d , capcity : %d" , len(s1),cap(s1)) //lenght : 2 , capcity : 6
```



slice literal

```go
sli := []int{1,2,3} //we didn't put anything in braclet than compiler treat this as a slice
```



```make([]T ,len)```

```go
func main() {
	s := make([]int,3,4)

	s[0] = 1
	s[2] = 2
	s[3] = 3

	s = append(s, 4)
	s = append(s, 5)
}

/*

panic: runtime error: index out of range [3] with length 3

*/
```



```go
s := make([]int,3)

s[0] = 1
s[2] = 2
s[3] = 3

s = append(s,4) //increase slice
s = append(s,5,6,7,8) //increase slice manyvalues

fmt.Printf(s[1:3]) // 2 3 4
fmt.Printf(s[:3]) // 1 2 3
fmt.Printf(s[1:]) // 2 3 4 5 6 7 8


s[1] = 100 // will effect all slice that common


//if you want to deep copy a slice
x := make([]int,len(s))
copy(x,s)

//but if you will append this will copy the array and create new one

//2d dlice
twoDimentionSLice := make([][]int,3)
// [ [0] , [1,2] , [3,4,5] ]
for i:=0; i<3; i++ {
    inner_len := i+1
    ss[i] = make([]int,inner_len)
    
    for j:=0; j<inner_len; j++ {
		twoDimentionSLice[i][j] = i + j
    }
}

```



maps

key value pairs

```go
var idMap map[string]int
// map[key]value

var idMap map[string]int
idMap = make(map[string]int)

//map literal
idMap := map[string]int {
    "joe" : 123,
    "haim" :122222
}

//fetch values
fmt.Println(idMap["joe"]) //123

//adding key value pair
idMap["jane"] = 456

// deleting a key value pair
delete(idMap,"joe") //get map and key

id, p := idMap["joe"] // id is value , p is boolean represent if found

// get length of map
fmt.Println(len(idMap))

//iterate map

for key,val := range idMap {
    fmt.Println("key : , value : ", key, val)
}
```



Struct

```go
type Person struct{
    name string
    address string
    phone string
}

var p1 Person
p1.name = "aviad"
fmt.Printf("person 1 name %d " , p1.name)

//initlize
p1 := new(Person)

p1:= Person(name: "joe",address: "a st." , phone: "05840")
```



## protocols and formats 

> > summary about buffers

RFCS

Requests for comments

definitions of Internet protocols and formats.

```go
http.Get('url') //send get request

p := Person(name: "joe", address: "saffsa",phone : "123")

// seriliazie
barr , err := json.Marshal(p1)

//desriliaze , json object has to have same fields name
var p2 Person
err := json.Unmarshal(barr,&p2)
```



files

```go
//open file

err := ioutil.WriteFile("outfile.txt",data,0777)
// src , data ,permissions

/*
os.Open()
os.Close()
os.Read()
os.Write()
*/

//READ 10 BYTES FROM A FILE
file , err := os.Open("dt.txt")
barr := make([]byte,10)
nb,err := file.Read(barr)
f.Close()

//Create/Write file
file,err := os.Create("outfile.txt")
barr := [...]byte{1,2,3}
nb,err := f.Write(barr)
nb,err := f.WriteString("Hello")

//read file

data,err := ioutil.ReadFile("text.txt") 
//read content of whole file as byte , this will close file automaticliy 
//issue when reading large file
//this will read the file on ram , can cause memory leak , not recommend with large files

//write


//close when you done with file


//seek , move read/wrtie head
```



# Functions

 

you can pass couple arguments from same type without mention the type all the time

```go
func go(x int , y int){}
// can be replaced with
func go (x,y int){}
```



return value

```go
func foo(x int) string {} //this will get int and return string

//if you return multiple values

func foo(x int) {string,int} {} //this will get int and return string and int
```



Call by Value

```go
func foo(y int){
    y=y+1
}

func main(){
    x:=2
    foo(x)
    fmt.Print(x) 
    /*
    this will print 2 and not 3 , if you want to change the value you can 					pass the pointer.
    if we want to pass big object/slice we don't want to let go copy this big instance to 	  the function , then we'll pass pointer to this object 
    */
}
```

call by refrence

```go
func foo(y *int){
    *y = *y+1
}

func main(){
    x:=2 
    foo(&x)
    fmt.Print(x) //will print 3 
}
```



```go
func foo(x *[3]int) {
    (*x)[0] = (*x)[0] + 1
}

func main(){
    a := [3]int{1,2,3}
    foo(&a)
    fmt.Print(a)
}

//this is messy we can use slice instead , slice is not actally array this is pointer to the array

func foo(sli int) {
    (*x)[0] = (*x)[0] + 1
}

func main(){
    a := []int{1,2,3}
    foo(a)
    fmt.Print(a)
}

```



 

## Functional Programing in GO

treat function as any other type (int,float,boolean,etc...)

you can set variable equal to function

the function will be created dynamically while code is running

we can pass to function , function as argument

we can save function in data structure

```go
func main(){
	 inc := func(z int) int {
		return z+1
	}
    
	fmt.Print(inc(1))
}
```

```go
func applyIt(afunct func (int) int , val int) int {
    return afunct(val)
}

func incFn(x int) int {return x+1}
func decFn(x int) int {return x-1}

func main(){
    fmt.Println(applyIt(incFn,2)) //3
    fmt.Println(applyIt(decFn,2)) //1
}
```



anonymous functions

functions without a name

```go
func applyIt(afunct func (int) int , val int) int {
    return afunct(val)
}

func main(){
    number := applyIt(func(x int) int {return x+1} , 2)
    fmt.println(number) //3
}
```



functions can return other functions.

let's create a function that create a function that give you the distance between point to other point.



```go
func MakeDistanceOrigin(originX,OriginY float64) func(float64,float64) float64{
    return func(x,y float64) float64 {
        const xPow := math.Pow(x-originX,2)
        const yPow := math.Pow(y-originy,2)
        return math.Squrt(xPow + yPow)
    }
}

func main(){
    distanceFromCar := MakeDistanceOrigin(34.156561,31.516651)
    distanceFromJob := MakeDistanceOrigin(38.156561,36.516651)
    fmt.Printf("my distance from home is %d",distanceFromJob(11.51656,12.1231))
}
```



Closure

Clousre = Function + its environment

 when you pass function as argument you actually pass his environment also.

```go
func fA() func() int {
    i := 0
    return func() int {
        i++
        return i
    }
}
func main() {
   fB := fA()
   fmt.Print(fB())
   fmt.Print(fB())
}
```



this will output 12

notice that i will be saved and change when increment



Varaidc and Defered



when you don't know how many arguments will be passed and you want to be flexible you can use `...`

in function you will get this as a slice.

```go
func getMax(numbers ...int) int {
    maxNumber := -1
    for _ , value := range numbers {
        if value > maxNumber{
            maxNumber = value
        }
    }
    reutrn maxNumber
}

func main(){
    fmt.Println("Max number is " , getMax(1,2,3,4,8,80,90))
    
    //also can pass a slice
    numbers := []int{1,5,6,7}
    fmt.Println("Max number is " ,numbers...) 
}
```



Deffered function - will execute function later , 

```go
func main(){
    defer fmt.Println("Bye!")
    fmt.Println("Hello!")
}

//output => Hello! Bye!
```

the arguments passed imminently but function will be evoked later

```go
func main() {
    i:=1
    defer fmt.Println(i)
    i=i+1
    fmt.Println("Hello!")
}

//output Hello! 1
```



## Classes and Encapsulation



