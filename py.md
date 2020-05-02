# Python 

## OOP

```python
class Employee:
    pass

emp_1 = Emplpyee()
emp_1 = Emplpyee()
```



`pass` will allow create empty functions and class



we can set properties that don't exists

```python
emp_1.email = "someemail@email.com"
```



constructor method is `__init__` 

always the first argument of `__init__` will be the instance that we are creating.

we don't need excitingly pass the first argument this is occur implicitly.

the convention is to name the instance as `self` 

```python
class Employee:
    def __init__(self,first,last,pay):
        self.first = first
        self.last = last
        self.pay = pay
        self.email = first + '.' + last + '@company.com'

emp_1 = Emplpyee('avi','hayu',132) # init will run automaticly
emp_2 = Emplpyee('test','blabla',6000) # init will run automaticly
```



we can access the variable by 

```python
print(emp_1.first)
```



add methods to class

```python
class Employee:
    def __init__(self,first,last,pay):
        self.first = first
        self.last = last
        self.pay = pay
        self.email = first + '.' + last + '@company.com'
    
    def fullname(self):
        retur '{} {}'.format(self.first,self.last)

emp_1 = Emplpyee('avi','hayu',132) # init will run automaticly
print( emp_1.fullname() )

# we also can call the method by

Emplyee.fullname(emp_1)
```



​	

we can global variable 

```python
class Employee:
    num_of_emps = 0 # this will be class varible
    raise_amount = 1.04 # this will be instance varible
    
    def __init__(self,first,last,pay):
        self.first = first
        self.last = last
        self.pay = pay
        self.email = first + '.' + last + '@company.com'
    	
        Employee.num_of_emps += 1 # for class varible we use name of class
    
    def fullname(self):
        retur '{} {}'.format(self.first,self.last)
        
    def apply_raise(self):
        self.pay = int(self.pay * self.raise_amount) # for instance varible we use self
       

emp_1 = Emplpyee('avi','hayu',132) 
emp_2 = Emplpyee('test','blabla',6000) 
print(Employee.num_of_emps)
```



if we need to override global value like `num_of_emps` we should not use self but the name of the class.`	`



`classmethods` and `staticmethods`.

 Class methods are methods that automatically take the class as the first argument. Class methods can also be used as alternative constructors. Static methods do not take the instance or the class as the first argument. They behave just like normal functions, yet they should have some logical connection to our 

```python
class Employee:
    num_of_emps = 0 # this will be class varible
    raise_amount = 1.04 # this will be instance varible
    
    def __init__(self,first,last,pay):
        self.first = first
        self.last = last
        self.pay = pay
        self.email = first + '.' + last + '@company.com'
    	
        Employee.num_of_emps += 1 # for class varible we use name of class
    
    @classmethod
    def from_string(cls,employee_str,delimiter):
        first,last,pay = employee_str.split(delimiter)
        return cls(first,last,pay)
    
    @staticmethod
    def is_workday(day):
        return day.weekday() == 5 or day.weekday() == 6
    
new_emp1 = Employee.from_string('Aviad-Ha-2')

my_date datetime.date(2020,7,11)
Employee.is_workday(my_date)
```



inheritance

```python
# Developer will have all attrubitus and methods of Employee
class Developer(Employee):
    def __init__(self,first,last,pay,prog_lang):
        
        #usally in use when inherit more than one parent
        #Employee.__init__(self,first,last,pay) 
        
        super().__init__(first,last,pay)
        self.prog_lang = prog_lang
        
        
    
```

if class don't have` __init__` will go to the parent , if parent won't have will go to the parent of all classes named `object`.



if we need to check type in runtime we have 2 beautiful functions :

- `issubclass(class1,class2)`
- `isinstance(class1,class2)`



Dunder

when we add two numbers/string/dates etc..

what happend behind the scens is that the method `__add__` is called 

```python
sum = 1 + 7
# is equivilant to
sum = init.__add__(1,7)

print(sum)
# is equivilant to
print(sum.__str__)

len('Avi')
# is equivilant to
'Avi'.__len__
```

to create methods like this we need to check what the name of method is required and implement this in our class.



for example 

```python
class Employee:
    
    def __init__(self,first,last,pay):
        self.first = first
        self.last = last
        self.pay = pay
        self.email = first + '.' + last + '@company.com'

    def __add__(self,other):
        return self.pay + other.pay
    
    
emp_1 = Employee('test1_first','test1_last',10)
emp_2 = Employee('test2_first','test2_last',8)

emp_1 + emp_2 # equal to 18
```



Property Decorator

```python
class Employee:
    
    def __init__(self,first,last,pay):
        self.first = first
        self.last = last
        self.pay = pay

    def fullname(self):
        return '{} {}'.format(self.first,self.last)

```

what happens if we want to handle fullname as a property and not as an function ?

we can add annotation of property 



```python
@property
def fullname(self):
    return '{} {}'.format(self.first,self.last)
```



now we have get method.

what if we want to set employee fullname ?

we can use `@` + `prop_name` + `.` + `settet`

for fullname we'll use `@fullname.setter`



```python
@fullname.setter
def fullname(self,name):
    first,last = name.split(' ')
    self.first = first
    self.last = last
```



```python
class Employee:
    
    def __init__(self,first,last,pay):
        self.first = first
        self.last = last
        self.pay = pay
        
	@property
    def fullname(self):
        return '{} {}'.format(self.first,self.last)
    
    @fullname.setter
	def fullname(self,name):
    	first,last = name.split(' ')
    	self.first = first
    	self.last = last

```



# args & kwagrs

like spread operator in python we can pass as much arguments as we want to an function when the not in array with `args` or `kwargs`



```python
site_title = 'My Blog'

def blog_posts(title,*args,**kwagrs):
    print(title)
    for x in args:
        print x
    for p_site,post in kwargs.items():
        print(p_title,post)
        
blog_posts('My Blog',#this for title
          '1','2','3', # this is for args
          blog_1 = 'Awsomwe blog', # this for kwargs
          blog_2 = 'Python is fun')

```

args is like an array

kwagrs is like a dictionary





# Iterators and Iterables

