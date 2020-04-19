

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

Closure = Function + its environment

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

```go
type MyInt int

func (mi MyInt) DoubleMyInt () int {
    return int(mi*2)
}

func main() {
    v := MyInt(3)
    fmt.Println(v.DoubleMyInt())
}
```



to connect method and types we have to use reviver

when we look at function ```DoubleMyInt()``` the reviver of the function is ```(mi MyInt)```

now we can use the method with this type , notice that we are doing ```v.DoubleMyInt()```



when we connect between method and types we have that both will be in same package.

what happens behind the scenes is the `MyInt` transfered to the function `DoubleMyInt` as an argument , 

```DoubleMyInt(mi MyInt)```

this is call by value.

 that copy the entire object to the function.

if we not returning the object and just want to make a change on the object , this will be made on the copy and not on the original object.

usually we will pass a pointer so call by reference will be.





we can use the type as struct 

```go
package main

import (
	"fmt"
	"math"
)

type Point struct {
	x float64
	y float64
}

func (p *Point) DistToOrigin() float64 {
	f := math.Pow(p.x, 2) + math.Pow(p.y, 2)
	return math.Sqrt(f)
}

func main() {
	p1 := Point{3,4}
	origin := p1.DistToOrigin()
	fmt.Println(origin)
}
```



Encapsulation

when we create new package and we give to functions name that not starts with capital letter , this will be private.



## Polymorphism

golang does not have inheritance.

interface 

```go
type Shape2D interface {
    Area() float64
    Perimeter() float64
}

type Triangle {...}

func (t Triangle) Area() float64 {...}
func (t Triangle) Perimeter() float64 {...}
```

in go we don't specify explicitly that some struct is satisfied an interface , we just implement the interface functions.

```go
package main

import "fmt"

type Speaker interface {
	Speak()
}

type Dog struct {name string}
func (d Dog) Speak() {
	fmt.Println(d.name)
}

type Cat struct {name string}
func (d Cat) Speak() {
	fmt.Println(d.name)
}

func main(){
	var dog Speaker =  Dog{"Alf"}
	var cat Speaker =  Cat{"Cat"}

	speakers := []Speaker{dog,cat}

	for _, speaker := range speakers {
		speaker.Speak()
	}
}
```



in go , if instance is not initialized and he is implement interface , the calling of the functions of the interfaces will be still available

```go
func (d *Dog) Speak() {
    if d == nil {
        fmt.Print("<noise>")
    }else{
        fmt.Println(d.name)
    }
}

func main() {
    var s1 Speaker
    var d1 *Dog
    s1 = d1
    s1.Speak()
}

//output <noise>
```



if you want to specify function that will accept any type as a parameter use the empty interface

```go
func PrintMe(val interface{}){
    fmt.Println(val)
}
```



type assertions 

```go
func DrawShape (s Shape2D) bool {
    rect, ok := s.(Rectangle)
    if ok {
        DrawRect(rect)
    	return
    }
    tri, ok := s.(Triangle)
    if ok {
        DrawTriangle(tri)
 		return
    }
    DrawRegularShape(s)
}
//or
func DrawShape (s Shape2D) bool {
    switch:= sh:= s.(type) {
        case Rectengle:
        	DrawRect(sh)
    	case Triangle:
           DrawTriangle(sh)
   	   default :
          DrawRegularShape(sh)
    }
}
```



The Error Interface

```go
type error interface{
    Error() string
}
```





# Course 3 - Concurrency

parallel is to execute 2 or more things in the same time

At a time t an instruction is being performed for both core 1 and core 2

you have to have more than one core/processors/cpu to allow to allow parallel.

Parallel is about speedup.

some tasks are not parallelize 



Why use concurrency ?



Von Neumann Bottleneck 

in the past (5 years old) software has been optimized by get new faster processors 

CPU use ram , ram always slower than CPU then bottleneck is created.

 what people have done to prevent this is using cache on the chip to speed up things.

this is change for couple reasons :



Moore's Law

this is an observation

Predicted that transistor density would double every two years

Increase of destiny of transistor would lead to increase in speed



power wall

Transistors consume power when they switch 

Increasing transistor density leads to increased power consumption.

smaller transistors use less power but density scaling is much faster

high power leads to high temperature 

usually the processor is being cool because there is a high temperature and we don't want it be melt

```P = α * CFV²```

`α` is percent of time switching 

`c` relate to size of transistor

`f` is the clock frequency 

`v` voltage from low to high (example 0 to 5 Volt)



Dennard Scaling

when transistor gets smaller voltage gets smaller

then power consumption and temperature is low.

the problem is that we cannot put the voltage too low because we have threshold

then small the transistor and put more is not relevant. 



we cannot scale more than that without let the hardware melt.



So how do we scale ? 

How we can optimize everything ?



increase number of cores this trend is apparent today.

code made to execute on multiple cores.

concurrency is to tell the program how it's going to be divided amongst cores.



## Process 

process is an instance of running program (like running power point)

things unique to a process :

- Memory
  - code
  - stack 
  - heap
  - shared libraries
- Registers 
  - Program counter , data regs ,stack ptr...

Processes are switched quickly each process run 20 ms 

Operating system must give processes fair access to resources



Scheduling

Operating system schedules processes for execution.

This gives illusion of parallel execution (if we have one core)

we have some algorithm for scheduling , for example we have round robin that will give each process 20ms.

sometimes we give to process higher priority , give more time to some processes 

when os switch to another process this called context switching

when we doing context switching we pause the state(memory,registers) of process 

when we resume process we take his state and continue after x time pause again 



Thread vs Processes

Context switching can take time because data will be taken to the memory , reading stuff from memory back into registers , this is a lot of memory accesses and this is can be slow.



Threads is light weight processes , Thread is a like process but it shares some of the contexts with other threads in the process 

on process can have multiple threads.



![image-20200415230344540](/home/aviad/.config/Typora/typora-user-images/image-20200415230344540.png)



We can notice that we have process with 3 threads 

each thread contain his own data (Stack,registers,code) but they also share data.

these threads have unique context but is much less than the unique context between different processes.

context switch between **threads** is fast.



OS instead scheduling processes is scheduling threads by threads 



Threads and Gorounites

go routine is a thread in go

many  go routines execute within a single OS thread 

OS schedule Main Thread , and main thread is switching the threads 

![image-20200415230935298](/home/aviad/.config/Typora/typora-user-images/image-20200415230935298.png)



Go run time scheduler it does scheduling within an OS thread  

Goruntime chose different Goroutine each time  

Goruntime using logical processor that mapped to a thread.

if we want to allow parallel we can put another Logical Processor according to cores , 

then each logical processor will be mapped to different OS thread in each core 

the programmer cannot determine which Goroutine will be in which Goruntime but can control of number of cores for go (default will be 1).

![image-20200415222511050](/home/aviad/.config/Typora/typora-user-images/image-20200415222511050.png)





## Interleaving

Concurrent code is non-deterministic 

the code can run each time in different order

![image-20200415232817612](/home/aviad/.config/Typora/typora-user-images/image-20200415232817612.png)

two possibilities of running

![image-20200415232835172](/home/aviad/.config/Typora/typora-user-images/image-20200415232835172.png)

this is not happen in go level , but in the machine code level.



## Race Conditions 

outcome depends on non-deterministic ordering

![image-20200415233231173](/home/aviad/.config/Typora/typora-user-images/image-20200415233231173.png)

Race occur due to communication 



## GOROUTINE

one Goroutine is automatically created to run the main function

other are created using to go keyword

```go
func foo(){
    fmt.Println("foo")
}

func main(){
    a := 1
    go foo()
    a = 2
}
```

this create new thread then `foo` is executed in this thread.

a go routine is exists when is code complete.

when the main is complete all other GoRoutine exit

```go
func main(){
    go fmt.Printf("New routine")
    fmt.Printf("Main routine")
}
```

what will occur is that only Main routine will be display , because the main routine finish the code and close all other GoRoutine.



Bad practice that handle this issue is to block the main thread by sleeping , go won't let time waist than he would go to other thread.

one thread is waiting the scheduler will move to another thread.

```go
func main(){
    go fmt.Printf("New routine")
    time.Sleep(100 * time.Millisecond)
    fmt.Printf("Main routine")
}
```

this is bad because we can not assume time to sleep that is ensure finish of another thread.

the OS may schedules another thread when we specify sleep

time is non deterministic



```go
func main(){
    x := 1
	go func(){
        x+=1
		fmt.Println(x)
	}()    
    x+=1
    fmt.Println(x)
}

//this code is non deterministic , if we would like to print this when x = 1

func main(){
	x := 1
	wg := sync.WaitGroup{}
	wg.Add(1)
	go func(){
		x++
		fmt.Println(x)
		wg.Done()
	}()
	wg.Wait()
	x++
	fmt.Println(x)
}

```

now we can control synchronization of threads  

synchronization will be used to restrict bad interleaving

`waitGroup` force GoRoutine wait to other GoRounites

when we using `wg.Wait()` we wait that counter will be 0

when we do `wg.add()` we increment the counter , and when we `wg.Done()` we decrement the counter.



## Communication GoRounites

transfer data between Gorounites is by channels

channels are typed 

```go
c := make(chan int) //create channel
c <- 3 //send data
x := <- c //recive data from channel
```



```go
func prod(num1 , num2 int ,channel chan int){
    channel <- num1*num2
}

func main() {
    channel := make(chan int)
    go prod(1,2,channel)
    go prod(3,4,channel)
    a := <- channel
    b := <- channel
    fmt.Println(a*b)
}

//output 24
```

Blocking on channels 

channel default state is unbuffered `make(chan int) ` when we not pass another argument to the make.

sending blocks until data is received

receiving blocks until data is sent

![image-20200417030008217](/home/aviad/.config/Typora/typora-user-images/image-20200417030008217.png)

task1 will wait until task2 gets the data.

as well the task2 will block the code until gets data

blocking is the same as waiting communication 



![image-20200417030521722](/home/aviad/.config/Typora/typora-user-images/image-20200417030521722.png)

in this code task2 will block until task1 will send data , we don't assign the data thats going on the channel , this is like alternative using wait group.



Buffered Channels

unbuffered channel have no capacity but channel can have limited number of objects 

```go
channel := make(chan int , 3)
```



now sending only blocks when channel is full

receiving only blocks if buffer of channel is empty



let's assume we have channel with capacity 2

![image-20200417031227732](/home/aviad/.config/Typora/typora-user-images/image-20200417031227732.png)

task1 write int to channel , 

then task2 gets the number

then task2 wait to another number forever (assume task1 not writing anymore to channel)



another example

![image-20200417031510776](/home/aviad/.config/Typora/typora-user-images/image-20200417031510776.png)

task1 will send data

then task2 will receive data

task1 will send data

task2 not going to receive data anymore

task1 is blocked forever , waiting someone take his number



#### main use of buffering 

producer faster than consumer

let's assume that we have Goroutine that produce data to channel , and Goroutine that consume the data

let's assume that producer is faster than consumer

then when we using buffer we allow some delay with the producer and consumer.

this allow continue executing without blocking until going to some threshold (capacity of channel)

![image-20200417032022551](/home/aviad/.config/Typora/typora-user-images/image-20200417032022551.png) 





to continue read from channel we can 

```go
for value := range channel {
    fmt.Println(value)
}
```

this will quit the loop when we'll close the channel

```go
close(channel)
```

when you using for , and the producer know that he won't pass data he need to call close.



Receiving from multiple Gorounites

![image-20200417160212976](/home/aviad/.config/Typora/typora-user-images/image-20200417160212976.png)

let's assume that t3 need to gets data from t1 and t2 



```go
num1 := <- c1
num2 := <- c2
fmt.Println(a*b)
```

this are blocking , num1 will block until gets data



when we want data either c1 or c2

```go
select {
    case a = <- c1 :
    	fmt.Println(a)
    case b = <- c2 :
    	fmt.Println(b)
}
```



we can either use it with send and receive

```go
select {
    case a = <- inchan:
    	fmt.Println("received a")
	case outchan <- b :
    	fmt.Println("sent b")
}
```



common use is to use abort channel when we want to exit.

```go
for {
    select {
        case a <- c:
        	fmt.Println(a)
    	case <- abortChannel :
        	return
    }
}
```

abort channel will let us exit infinite loop.



another way not to block is using default

```go
select {
	case a = <- c1:
    	fmt.Println(a)
    case b = <- c2:
    	fmt.Println(b)
    default:
    	fmt.Println("nop")
}
```



sharing variables in threads

```go
package main

import (
	"fmt"
	"sync"
)

var i = 0
var wg sync.WaitGroup

func inc(){
	i = i +1
	wg.Done()
}

func main()  {
	wg.Add(2)
	go inc()
	go inc()
	wg.Wait()
	fmt.Println(i)
}
```

what can occur here is that both inc will run in the same time and they both increment 1.

the output will be 2 instead of 3.

running this , will give different output because the interleaving 

to prevent this we'll use mutex

```go
package main

import (
	"fmt"
	"sync"
)

var i = 0
var wg sync.WaitGroup
var mut sync.Mutex

func inc(){
    mut.Lock()
	i = i +1
    mut.Unlock()
	wg.Done()
}

func main()  {
	wg.Add(2)
	go inc()
	go inc()
	wg.Wait()
	fmt.Println(i)
}
```

we'll assure that two thread cannot write both to same shared variable.

mutex use the binary semaphore , if flag up shared variable is in use , flag down available to use

![image-20200417164042198](/home/aviad/.config/Typora/typora-user-images/image-20200417164042198.png)

before using shared variable we will `Lock()` then when we finish we'll `Unlock()` 

if another thread will try to access while shared variable is being locked 

the lock will block until unlock.



### Synchronous Initialization

when we want to initial some object , 

we could perform the initial before starting with all others GoRounites but what happens when we initial when the Gorounites is actually up and working 

we can use `Sync.Once` `once.Do(function)` 

function will executed only one time even if exists in multiple go routines 

all calls for `once.Do(function)`will block until the first returns 

```go
var wg sync.WaitGroup

var on sync.Once

func setup(){
    fmt.Println("Init")
}

func dostuff(){
    on.Do(setup)
    fmt.Println("hello")
    wg.Done()
}

func main() {
    wg.Add(2)
    go dostuff()
    go dostuff()
    wg.Wait()
}

/*
	setup will called only one time
	output :
				Init
				hello
				hello
*/
```



### Deadlock

when GoRoutine 1 depend on GoRoutine 2 and GoRoutine 2 depend on GoRoutine 1

can occur cause waiting channel , locking , unlocking etc...

example for dead lock

```go
func dostuff(c1 chan int , c2 chan int){
    <- c1 //wait to receive
    c2 <- 1 // write to another channel
    wg.Done() //decrement wg count
}

func main(){
    ch1 := make(chan int)
    ch2 := make(chan int)
	
    wg.Add(2)
    go dostuff(ch1,ch2)
    go dostuff(ch2,ch1)
    wg.Wait()
}
```

go runtime can detect this and stopped with an error.

cannot detect when a subset of Gorounites are deadlocked.



## Dining Philosophers Problem

![image-20200417182113560](/home/aviad/.config/Typora/typora-user-images/image-20200417182113560.png)

![image-20200417193601884](/home/aviad/.config/Typora/typora-user-images/image-20200417193601884.png)

```go
type ChopS struct { sync.Mutex }
type Philo struct { leftCS,rightCS *ChopS}
var wg sync.WaitGroup

func (p Philo) eat(){
    for {
        p.leftCS.Lock()
        p.rightCS.Lock()
    
        fmt.Println("eating")
        
        p.leftCS.Unlock()
        p.rightCS.Unlock()        
    }
    wg.Done()
}

func main(){
    CSticks := make([]*ChopS,5)
    for i:=0; i<5; i++ {
        CSticks[i] = new(ChopS)
    }
    
    philos := make([]*Philo,5)
    for i:=0; i<5; i++ {
        philo[i] = &Philo{
            Csticks[i],
            Csticks[(i+1)%5]
        }
    }

    wg.Add(5)
    for _,p := range philos {
        go p.eat()
    }
    wg.Wait()
}

```





