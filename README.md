[CSE111_Notion_Cheat_Sheet.md](https://github.com/user-attachments/files/32113069/CSE111_Lab12_Notion_Cheat_Sheet.md)
# CSE111 Programming Language II ---Cheat Sheet by Samia Rahman Mridula
   
> **Exam-focused Notion notes** based on the CSE111 Lab 12 Practice
> Sheet.\
> Main focus: OOP, inheritance, class variables, method overriding,
> operator overloading, lists/dictionaries, `*args`, scope tracing, and
> output tracing.

------------------------------------------------------------------------

# 1. Class & Object

## Basic structure

``` python
class Student:
    def __init__(self, name, ID):
        self.name = name
        self.ID = ID

s1 = Student("Bob", "20301018")
```

-   **Class** = blueprint
-   **Object** = instance of a class
-   `self` = current object
-   `self.name` and `self.ID` belong to the object

------------------------------------------------------------------------

# 2. Constructor `__init__()`

`__init__()` runs automatically when an object is created.

``` python
class Account:
    def __init__(self, name, balance):
        self.name = name
        self.balance = balance
```

``` python
a1 = Account("Rahim", 5000)
```

Execution:

``` text
__init__() runs
↓
self.name = "Rahim"
self.balance = 5000
```

------------------------------------------------------------------------

# 3. Instance Variable vs Class Variable

## Instance variable

Different for every object.

``` python
self.name
self.balance
self.x
```

Example:

``` python
a1.balance = 5000
a2.balance = 10000
```

## Class variable

Shared by the class.

``` python
class Student:
    total = 0
```

Access:

``` python
Student.total
```

or sometimes:

``` python
self.total
```

### IMPORTANT

If the question says **total number**, **counter**, **database**, or
**shared information**, think **class variable**.

The Student task explicitly requires class/static variables for total
students and department counts.

------------------------------------------------------------------------

# 4. `self`

`self` means **the current object**.

``` python
self.x
```

means:

> the `x` belonging to this object.

Example:

``` python
q1 = Q5()
q2 = Q5()

q1.x = 10
q2.x = 20
```

Then:

``` python
q1.x   # 10
q2.x   # 20
```

------------------------------------------------------------------------

# 5. Local Variable vs Instance Variable

``` python
def methodA(self):
    x = 5
    self.x = 10
```

-   `x` → local variable
-   `self.x` → instance variable

Local variable disappears after the method finishes.

Instance variable stays inside the object.

### Exam rule

``` python
x = ...
```

→ local

``` python
self.x = ...
```

→ object state changes

------------------------------------------------------------------------

# 6. Class Variable Access

Example:

``` python
class A:
    temp = 5

a = A()
```

These refer to the class variable when no instance variable shadows it:

``` python
A.temp
self.temp
```

But:

``` python
self.temp = 10
```

creates/changes an **instance attribute** for that object.

It does NOT necessarily change:

``` python
A.temp
```

### VERY IMPORTANT

``` python
A.temp += 1
```

→ changes class variable

``` python
self.temp += 1
```

→ normally changes/creates the object's instance variable

This distinction is central to Tasks 19, 22, and 24.

------------------------------------------------------------------------

# 7. Counter Pattern

Used in Tasks such as `Student`, `Account`, `Transport`, `Fruit`, and
player database problems.

``` python
class Student:
    total = 0

    def __init__(self):
        Student.total += 1
```

If:

``` python
s1 = Student()
s2 = Student()
s3 = Student()
```

then:

``` python
Student.total
```

is:

``` text
3
```

### Department counter

``` python
Student.total += 1

if dept == "CSE":
    Student.cse += 1
elif dept == "BBA":
    Student.bba += 1
```

------------------------------------------------------------------------

# 8. Methods

A method is a function inside a class.

``` python
class Account:
    def addMoney(self, amount):
        self.balance += amount
```

Call:

``` python
a1.addMoney(3000)
```

The object `a1` is automatically passed as `self`.

------------------------------------------------------------------------

# 9. Optional Arguments / Default Arguments

Very important for Task 1.

``` python
def calculateTotal(self, earning, goals=None):
```

Now both are possible:

``` python
player.calculateTotal(250000)
```

and:

``` python
player.calculateTotal(250000, 31)
```

Check:

``` python
if goals is None:
    bonus = 0
```

### General pattern

``` python
def method(self, x=None):
    if x is None:
        # one-argument case
    else:
        # two-argument case
```

------------------------------------------------------------------------

# 10. Conditional Calculation

Task 1 pattern:

``` python
if goals is None:
    bonus = 0
elif goals > 30:
    bonus = 0.05 * earning + 10000
else:
    bonus = 0.05 * earning

total = earning + bonus
```

### Remember

``` text
No goals → bonus = 0

Goals > 30
→ 5% earning + 10000

Goals ≤ 30
→ 5% earning
```

------------------------------------------------------------------------

# 11. Inheritance

Inheritance means a child class gets features from a parent class.

``` python
class Child(Parent):
    pass
```

Example:

``` python
class Processor:
    def __init__(self, model, thread, core):
        self.model = model
        self.thread = thread
        self.core = core

class Intel(Processor):
    pass
```

Now:

``` python
p = Intel("Intel i5", 12, 6)
```

can use the parent constructor.

------------------------------------------------------------------------

# 12. Parent → Child Pattern

Common exam structure:

``` python
class Parent:
    def __init__(self, x):
        self.x = x

class Child(Parent):
    def __init__(self, x, y):
        super().__init__(x)
        self.y = y
```

Think:

``` text
Parent
  ↓
Child
```

The child gets the parent's attributes/methods and can add its own.

------------------------------------------------------------------------

# 13. `super()`

`super()` accesses the parent class.

``` python
super().__init__()
```

means:

> Run the parent constructor.

Example:

``` python
class B(A):
    def __init__(self):
        super().__init__()
        self.x = 10
```

Execution:

``` text
B.__init__()
↓
A.__init__()
↓
back to B.__init__()
```

------------------------------------------------------------------------

# 14. Method Overriding

A child can replace a parent method.

``` python
class Parent:
    def review(self):
        print("Parent review")

class Child(Parent):
    def review(self):
        print("Child review")
```

``` python
c = Child()
c.review()
```

Output:

``` text
Child review
```

The child version is used.

This appears in the `fiction`, `nonfiction`, `Intel`, `AMD`, `Bus`,
`Train`, Apple-product, and player tasks.

------------------------------------------------------------------------

# 15. Calling an Overridden Parent Method

``` python
class B(A):
    def methodA(self):
        super().methodA()
```

This forces Python to use the parent's version.

### Important tracing rule

``` python
self.methodA()
```

→ use the object's resolved/overridden method

``` python
super().methodA()
```

→ use the parent implementation

------------------------------------------------------------------------

# 16. Method Resolution in Inheritance

For:

``` python
class B(A):
```

and:

``` python
b = B()
b.methodA()
```

Python first looks for:

``` text
B.methodA
```

If it exists → use B's method.

If it does not → look in A.

So:

``` text
b.methodA()
↓
B.methodA()
↓
possibly super().methodA()
↓
A.methodA()
```

------------------------------------------------------------------------

# 17. `__str__()`

`__str__()` controls what happens when an object is printed.

``` python
class Student:
    def __str__(self):
        return self.name
```

Then:

``` python
s = Student()
print(s)
```

uses:

``` python
s.__str__()
```

### Example

``` python
def __str__(self):
    return "Name: " + self.name
```

------------------------------------------------------------------------

# 18. `__str__()` with Object Data

Fruit task pattern:

``` python
def __str__(self):
    return self.Order_ID + ", Weight: " + str(self.weight)
```

When:

``` python
print(m1)
```

Python calls:

``` python
m1.__str__()
```

------------------------------------------------------------------------

# 19. Operator Overloading

Operator overloading gives special meaning to operators for objects.

Common magic methods:

``` text
+   → __add__()
-   → __sub__()
*   → __mul__()
==  → __eq__()
<   → __lt__()
```

------------------------------------------------------------------------

# 20. `__add__()`

Example:

``` python
class Product:
    def __add__(self, other):
        return self.price + other.price
```

Then:

``` python
p1 + p2
```

means:

``` python
p1.__add__(p2)
```

### Fruit task

``` python
m1 + m2
```

should calculate the combined order price.

### Apple task

``` python
m1 + iphone
```

should return the combined total price.

------------------------------------------------------------------------

# 21. Inheritance + Operator Overloading

Typical pattern:

``` python
class Fruit:
    ...

class Mango(Fruit):
    def __add__(self, other):
        return self.total_price + other.total_price
```

Remember:

``` text
Inheritance
+
Overriding
+
Operator overloading
```

can appear in the same problem.

------------------------------------------------------------------------

# 22. Polymorphism

Different child classes can have the same method name but different
behavior.

Example:

``` python
p1 = Intel(...)
p2 = AMD(...)

p1.getInfo()
p2.getInfo()
```

Both use:

``` python
getInfo()
```

but each child can provide its own version.

------------------------------------------------------------------------

# 23. Lists Inside Objects

A class can store a list.

``` python
class myList:
    def __init__(self, *values):
        self.values = list(values)
```

Example:

``` python
l1 = myList(2, 3, 4, 5, 6)
```

Stored as:

``` python
self.values = [2, 3, 4, 5, 6]
```

Then:

``` python
sum(self.values)
```

------------------------------------------------------------------------

# 24. Variable Number of Arguments: `*args`

Task 2 uses variable-length values.

``` python
def __init__(self, *values):
    self.values = list(values)
```

Possible calls:

``` python
myList()
myList(1, 2)
myList(1, 2, 3, 4, 5)
```

Inside the function:

``` python
values
```

is a tuple.

Example:

``` python
def test(*args):
    print(args)
```

``` python
test(1, 2, 3)
```

gives:

``` text
(1, 2, 3)
```

------------------------------------------------------------------------

# 25. `*args` in Method Tracing

Task 23 uses:

``` python
def methodB(self, *args):
```

Then:

``` python
self.methodB(obj)
```

means:

``` python
len(args) == 1
```

while:

``` python
self.methodB(msg, msg[0])
```

means:

``` python
len(args) == 2
```

### Exam trick

Always count the arguments.

``` text
methodB(a)
→ args[0]

methodB(a, b)
→ args[0], args[1]
```

------------------------------------------------------------------------

# 26. `**kwargs`

General pattern:

``` python
def method(self, **kwargs):
    print(kwargs)
```

Call:

``` python
method(name="Bob", age=20)
```

`kwargs` becomes a dictionary:

``` python
{
    "name": "Bob",
    "age": 20
}
```

This is a useful general concept even though the main tracing tasks
focus more heavily on `*args`.

------------------------------------------------------------------------

# 27. Dictionaries

A dictionary stores key-value pairs.

``` python
data = {
    "Alice": 100,
    "Bob": 200
}
```

Access:

``` python
data["Alice"]
```

Add/update:

``` python
data["Charlie"] = 300
```

------------------------------------------------------------------------

# 28. Shared Class Dictionary

Player tasks use:

``` python
class Player:
    database = {}
```

Every player shares the same database.

Example:

``` python
Player.database["1LM10"] = [...]
```

All objects can access the shared database.

### Key idea

``` text
database = {}
```

inside the class → shared

``` text
self.database = {}
```

inside `__init__` → separate per object

------------------------------------------------------------------------

# 29. String Manipulation

Player ID tasks require extracting initials.

For:

``` python
name = "Lionel Messi"
```

``` python
parts = name.split()
```

gives:

``` python
["Lionel", "Messi"]
```

Initials:

``` python
parts[0][0] + parts[1][0]
```

→

``` text
LM
```

Player ID pattern:

``` text
player number + initials + jersey number
```

Example:

``` text
1LM10
```

------------------------------------------------------------------------

# 30. String Formatting

Useful styles:

``` python
"Name: {}".format(name)
```

or:

``` python
f"Name: {name}"
```

Example:

``` python
f"Price: {price}"
```

------------------------------------------------------------------------

# 31. `return` vs `print`

## `print()`

Displays something.

``` python
print(self.sum)
```

## `return`

Sends a value back.

``` python
return self.sum
```

Example:

``` python
x = methodB()
```

For this to work:

``` python
methodB()
```

must return something.

### Exam trap

Printing a value does NOT automatically return it.

------------------------------------------------------------------------

# 32. Object State Changes

If a method contains:

``` python
self.x += 5
```

the object's state changes permanently.

Example:

``` python
q.methodA()
q.methodA()
```

The second call starts with the state produced by the first call.

### NEVER reset object variables unless the constructor runs again.

------------------------------------------------------------------------

# 33. Method Calls Can Change Several Variables

Example:

``` python
def methodA(self):
    self.y += 5
    self.sum += self.y
    self.methodB()
```

`methodB()` may change:

``` text
self.x
self.y
self.sum
```

Therefore, when tracing:

1.  Enter method.
2.  Track local variables.
3.  Track `self` variables.
4.  Enter nested method.
5.  Apply its changes.
6.  Return.
7.  Continue the original method.

------------------------------------------------------------------------

# 34. Scope Tracing

Task 20 is mainly about scope.

Example:

``` python
def met1(self):
    x = 3
    x = self.x + 1
```

Here:

``` text
x       → local
self.x  → object attribute
self.y  → object attribute
```

If `met2()` changes:

``` python
self.x
self.y
```

those changes remain after `met2()` finishes.

But a local:

``` python
x
```

does not become `self.x`.

------------------------------------------------------------------------

# 35. How to Trace Difficult Code

For Tasks 19, 20, 21, 22, 23, and 24, make a table.

## Recommended table

  ----------------------------------------------------------------------------------
  Step    Object     Local x   Local y   `self.x`   `self.y`   `self.sum`      Class
                                                                            variable
  ------- -------- --------- --------- ---------- ---------- ------------ ----------
  1       q1             ---       ---        ---        ---          ---        ---

  2       q1             ---       ---        ---        ---          ---        ---

  3       q1             ---       ---        ---        ---          ---        ---
  ----------------------------------------------------------------------------------

Update the table **after every important statement**.

------------------------------------------------------------------------

# 36. Golden Rule: Three Types of Variables

When you see:

``` python
x
self.x
A.x
```

immediately classify them:

  Expression   Meaning
  ------------ ---------------------
  `x`          Local variable
  `self.x`     Instance variable
  `A.x`        Class variable of A

This single distinction solves a large part of the tracing questions.

------------------------------------------------------------------------

# 37. `self.method()` vs `Class.method()` vs `super().method()`

### `self.method()`

Use the method according to the object's class/inheritance resolution.

### `super().method()`

Use the parent implementation.

### `A.method(self, ...)`

Explicitly call A's method.

For inheritance tracing, pay special attention to these three forms.

------------------------------------------------------------------------

# 38. Mutable Objects and Aliasing

Very important in Tasks 21 and 23.

Suppose:

``` python
msg = []
myMsg = msgClass()

msg.append(myMsg)
```

Now:

``` python
msg[0]
```

and:

``` python
myMsg
```

refer to the **same object**.

Therefore:

``` python
msg[0].content = 10
```

also means:

``` python
myMsg.content == 10
```

### Mental model

``` text
msg
 ↓
[ object ]
    ↑
  myMsg
```

Changing the object through either reference changes the same object.

------------------------------------------------------------------------

# 39. Lists of Objects

Example:

``` python
msg = []
obj = msgClass()

msg.append(obj)
```

Then:

``` python
msg[0].content
```

accesses the object's `content`.

If:

``` python
obj.content = 50
```

then:

``` python
msg[0].content
```

is also `50`.

------------------------------------------------------------------------

# 40. `None`

Default optional argument:

``` python
def methodB(self, mg2=None):
```

Then:

``` python
methodB(obj)
```

means:

``` python
mg2 is None
```

But:

``` python
methodB(obj, msg)
```

means:

``` python
mg2 is not None
```

Typical pattern:

``` python
if mg2 == None:
    ...
else:
    ...
```

------------------------------------------------------------------------

# 41. GPA Calculation

Task 10 gives:

``` text
GPA =
sum(grade × course credit)
--------------------------
sum(course credit)
```

Each course has:

``` text
3 credits
```

So for equal-credit courses, GPA can also be viewed as the average of
the grade points.

Example:

``` text
3.3 + 3.0 + 4.0
---------------- = 3.43
       3
```

------------------------------------------------------------------------

# 42. Grade Conversion

Use the ranges exactly as given in the practice sheet:

       Marks   Grade Point
  ---------- -------------
         85+           4.0
      80--84           3.3
      70--79           3.0
      65--69           2.3
      57--64           2.0
      55--56           1.3
      50--54           1.0
    Below 50           0.0

### Coding pattern

``` python
if mark >= 85:
    grade = 4.0
elif mark >= 80:
    grade = 3.3
elif mark >= 70:
    grade = 3.0
...
else:
    grade = 0.0
```

------------------------------------------------------------------------

# 43. Price / Tax Pattern

Apple task:

``` text
Tax = base price × tax rate / 100

Total price = base price + tax
```

Example:

``` text
MacBook:
1299 × 10% = 129.9
1299 + 129.9 = 1428.9
```

``` text
iPhone:
799 × 5% = 39.95
799 + 39.95 = 838.95
```

Then:

``` text
1428.9 + 838.95 = 2267.85
```

------------------------------------------------------------------------

# 44. Bag Fee Pattern

Transport task:

``` text
0–2 bags → +0
3–5 bags → +60
More than 5 → +105
```

So:

``` python
if bags <= 2:
    fee = baseFare
elif bags <= 5:
    fee = baseFare + 60
else:
    fee = baseFare + 105
```

------------------------------------------------------------------------

# 45. Composition / Object Inside Another Object

A class can store another object.

Example:

``` python
msg = []
myMsg = msgClass()
msg.append(myMsg)
```

Now the list contains an object.

To access the object's attribute:

``` python
msg[0].content
```

This combination is common in tracing problems.

------------------------------------------------------------------------

# 46. Variable Shadowing

Example:

``` python
class A:
    temp = 5

    def method(self):
        self.temp = 10
```

After this:

``` python
A.temp
```

can still be:

``` text
5
```

while:

``` python
self.temp
```

is:

``` text
10
```

### Remember

An instance attribute can hide/shadow a class attribute for that object.

------------------------------------------------------------------------

# 47. Inheritance Tracing Order

For:

``` python
class B(A):
    def __init__(self):
        super().__init__()
        ...
```

Trace:

``` text
B object created
↓
B.__init__()
↓
super().__init__()
↓
A.__init__()
↓
A changes object/class state
↓
return to B.__init__()
↓
B continues
```

Do not skip the parent constructor.

------------------------------------------------------------------------

# 48. Nested Method Call Tracing

Example:

``` python
self.sum = x + self.methodB(x, y)
```

Do NOT calculate the whole line at once.

Trace:

``` text
1. Calculate x
2. Call methodB(x, y)
3. Trace methodB completely
4. Get methodB's return value
5. Calculate self.sum
```

If `methodB()` changes `self.y`, that changed value is used afterward.

------------------------------------------------------------------------

# 49. Task 1 --- PlayerEarning Pattern

Expected design:

``` python
class PlayerEarning:
    def __init__(self, name):
        self.name = name

    def calculateTotal(self, earning, goals=None):
        ...
```

Core logic:

``` text
No goals
→ bonus = 0

Goals > 30
→ bonus = 5% earning + 10000

Goals ≤ 30
→ bonus = 5% earning

Total = earning + bonus
```

------------------------------------------------------------------------

# 50. Task 2 --- `myList` Pattern

Need:

``` text
sum()
merge(...)
average()
```

Likely structure:

``` python
class myList:
    def __init__(self, *values):
        self.values = list(values)

    def sum(self):
        ...

    def merge(self, *values):
        ...

    def average(self):
        ...
```

Important edge case:

``` text
Empty list → Average: 0
```

------------------------------------------------------------------------

# 51. Task 3 --- Bird Pattern

Constructor stores:

``` text
name
can_fly
type
```

If no `can_fly` is provided, the output treats the bird as flightless.

Main concepts:

``` text
object state
conditional
default argument
setter method
```

------------------------------------------------------------------------

# 52. Task 4 --- Account Pattern

Class variable:

``` python
count = 0
```

Constructor:

``` python
Account.count += 1
```

Methods:

``` text
addMoney()
withdrawMoney()
printDetails()
```

Withdrawal must not make balance negative.

------------------------------------------------------------------------

# 53. Task 5 --- Smartphone Pattern

Core ideas:

``` text
optional name
dictionary for features
setName()
duplicate feature keys
```

Example:

``` text
Display: 6.2 inch, Amoled panel
Ram: 6 GB, DDR5
```

If phone name is missing:

``` text
Feature can not be added without phone name
```

------------------------------------------------------------------------

# 54. Task 6 --- Student Counter Pattern

Class/static variables:

``` text
total students
total CSE students
total BBA students
```

Each object needs:

``` text
overall serial
department serial
```

Example:

``` text
Naruto → overall 1, CSE 1
Sakura → overall 2, BBA 1
Shikamaru → overall 3, CSE 2
```

------------------------------------------------------------------------

# 55. Task 7--8 --- Simple Inheritance

### Book

``` text
book
├── fiction
└── nonfiction
```

Child classes override:

``` python
review()
```

### Processor

``` text
Processor
├── Intel
└── AMD
```

Child classes add:

``` text
price
```

and override:

``` python
getInfo()
```

------------------------------------------------------------------------

# 56. Task 9 --- Fruit

Inheritance:

``` text
Fruit
├── Mango
└── JackFruit
```

Important concepts:

``` text
class variable
__str__()
inheritance
__add__()
```

Price:

``` text
weight × unit price
```

------------------------------------------------------------------------

# 57. Task 10 --- CSEStudent

Inheritance:

``` text
Student
   ↓
CSEStudent
```

Child stores:

``` text
semester
courses
marks
```

Methods:

``` text
Details()
addCourseWithMarks()
showGPA()
```

Main exam concept:

``` text
marks → grade point → GPA
```

------------------------------------------------------------------------

# 58. Task 11 --- Transport

Inheritance:

``` text
Transport
├── Bus
└── Train
```

Class variable:

``` python
Transport.total_traveller
```

Every passenger increases the shared counter.

Bag fee depends on the number of bags.

------------------------------------------------------------------------

# 59. Task 12 --- AppleProduct

Inheritance:

``` text
AppleProduct
├── MacBookPro2020
└── iPhone12
```

Child classes override necessary methods.

Also implement:

``` python
__add__()
```

to combine total prices.

------------------------------------------------------------------------

# 60. Task 13 --- University

Inheritance:

``` text
University
├── CSE_dept
└── PHR_dept
```

Important concepts:

``` text
class variables
shared fees
instance data
__str__()
payment calculation
__add__()
```

### VERY IMPORTANT

The task changes:

``` python
University.admissionFee
University.Library
CSE_dept.PerCreditFee
...
```

after objects have already been created.

If payment uses class variables dynamically, **existing objects can show
the updated fees**.

------------------------------------------------------------------------

# 61. Task 14 --- Library

Inheritance:

``` text
Library
   ↓
Student
```

Shared class variables:

``` python
Library.Total_book
Library.borrow_data
```

Important concepts:

``` text
dictionary
shared state
inheritance
duplicate borrowing prevention
returning books
```

If Alice already borrowed a book, David cannot borrow the same book
until it is returned.

------------------------------------------------------------------------

# 62. Task 15 / 17 --- Player Database

Structure:

``` text
Player / Magical_SportsPerson
            ↓
 FootballPlayer / Quidditch_Player
```

Shared:

``` python
database = {}
playerNo = 0
```

Player ID:

``` text
player number + initials + jersey number
```

Example:

``` text
1LM10
2CR7
3MK11
```

The `createPlayer()` pattern is used to create a player while allowing a
retirement date.

------------------------------------------------------------------------

# 63. Task 18 --- User / Uber

Main concepts:

``` text
object state
conditional matching
route checking
ride type checking
status update
```

A passenger is picked only when the ride type and route requirements
match.

After pickup:

``` text
user.status()
```

shows the updated state.

------------------------------------------------------------------------

# 64. Task 19 --- Class Variable Tracing

Key lines:

``` python
Quiz1.temp = 4
```

and:

``` python
Quiz1.temp += 2
Quiz1.temp -= 1
Quiz1.temp += 1
```

These directly change the **shared class variable**.

When tracing:

1.  Write current `Quiz1.temp`.
2.  Create object.
3.  Apply constructor changes.
4.  Track `self.y` and `self.sum`.
5.  Trace `methodA()`.
6.  Enter `methodB()`.
7.  Apply class-variable changes.
8.  Return to `methodA()`.

------------------------------------------------------------------------

# 65. Task 20 --- Scope Tracing

Initial state:

``` text
self.x = 1
self.y = 100
```

Remember:

``` text
x     → local variable
self.x → object variable
self.y → object variable
```

`met2()` changes object state:

``` python
self.x = self.x + y
self.y = self.y + 200
```

So those changes carry into later method calls.

------------------------------------------------------------------------

# 66. Task 21 --- Object Reference Tracing

Important sequence:

``` python
myMsg = msgClass()
msg.append(myMsg)
```

Then:

``` python
msg[0]
```

and:

``` python
myMsg
```

point to the same object.

Therefore:

``` python
msg[0].content = ...
```

changes:

``` python
myMsg.content
```

too.

Also watch for:

``` python
methodB(msg[0])
```

versus:

``` python
methodB(msg[0], msg)
```

They enter different branches because of the optional second argument.

------------------------------------------------------------------------

# 67. Task 22 --- Inheritance + `super()` Tracing

Structure:

``` text
A
↓
B
```

Important lines include:

``` python
super().__init__()
```

and:

``` python
super().methodA(x, m)
```

Also watch:

``` python
A.temp
B.x
self.temp
self.y
self.sum
```

Do not assume they are the same variable.

------------------------------------------------------------------------

# 68. Task 23 --- `*args` + Aliasing

Structure:

``` python
def methodB(self, *args):
```

Branches:

``` python
if len(args) == 1:
```

and:

``` python
else:
```

Important:

``` python
msg[0]
args[0][0]
args[1].content
```

may refer to the same underlying object.

Track both:

``` text
Q5.x
self.x
```

because `Q5.x` is a class variable while `self.x` is an instance
variable.

------------------------------------------------------------------------

# 69. Task 24 --- Advanced Inheritance Tracing

Structure:

``` text
A
↓
B
```

Important concepts:

``` text
A.temp
B.x
self.temp
self.y
self.sum
super().__init__()
method overriding
optional parameter
nested method calls
```

Constructor flow:

``` text
B()
↓
B.__init__()
↓
super().__init__()
↓
A.__init__()
↓
back to B
```

Then check whether `obj` was supplied because:

``` python
B(obj)
```

takes a different constructor path from:

``` python
B()
```

------------------------------------------------------------------------

# 70. Common Exam Traps

## Trap 1 --- Confusing `self.x` and `A.x`

``` python
self.x
```

≠

``` python
A.x
```

unless no instance attribute shadows the class variable.

------------------------------------------------------------------------

## Trap 2 --- Forgetting constructor execution

Creating:

``` python
b = B()
```

automatically runs:

``` python
B.__init__()
```

and possibly:

``` python
super().__init__()
```

------------------------------------------------------------------------

## Trap 3 --- Forgetting state persists

``` python
q.methodA()
q.methodA()
```

The second call uses the state left by the first call.

------------------------------------------------------------------------

## Trap 4 --- Ignoring nested method calls

If you see:

``` python
self.methodB(...)
```

stop and trace `methodB()` before continuing.

------------------------------------------------------------------------

## Trap 5 --- Assuming `print()` returns a value

``` python
print(x)
```

does not mean:

``` python
return x
```

------------------------------------------------------------------------

## Trap 6 --- Missing object aliasing

``` python
msg.append(myMsg)
```

means `msg[0]` refers to `myMsg`.

------------------------------------------------------------------------

## Trap 7 --- Missing `*args` branch

Count arguments before deciding which branch executes.

------------------------------------------------------------------------

## Trap 8 --- Changing class variable accidentally

``` python
self.temp += 1
```

is not the same as:

``` python
A.temp += 1
```

------------------------------------------------------------------------

# 71. Output-Tracing Checklist

When asked **"What is the output?"**, follow this order:

### Step 1

Write initial class variables.

### Step 2

Create each object one by one.

### Step 3

Trace each constructor.

### Step 4

Record every instance variable:

``` text
self.x
self.y
self.sum
```

### Step 5

Record every class variable:

``` text
A.temp
B.x
Q5.x
```

### Step 6

For every method call, trace line-by-line.

### Step 7

For nested method calls, completely finish the nested method first.

### Step 8

Record every `print()` in exact order.

### Step 9

Record return values separately.

### Step 10

Only then write the final output.

------------------------------------------------------------------------

# 72. One-Page Memory Sheet

## OOP

``` text
class = blueprint
object = instance
self = current object
__init__ = constructor
```

## Variables

``` text
x       = local
self.x  = instance
A.x     = class variable
```

## Inheritance

``` python
class B(A):
```

## Parent

``` python
super().__init__()
super().method()
```

## Override

``` python
def sameMethod(self):
```

in child replaces parent behavior.

## String

``` python
__str__()
```

controls:

``` python
print(object)
```

## Operator

``` python
__add__()
```

controls:

``` python
obj1 + obj2
```

## Variable arguments

``` python
*args
```

→ tuple

``` python
**kwargs
```

→ dictionary

## Dictionary

``` python
data[key] = value
```

## Object state

``` python
self.x += 1
```

persists after method ends.

## Optional argument

``` python
def method(self, x=None):
```

## `None`

``` python
if x is None:
```

## Tracing

``` text
class variable
↓
constructor
↓
instance state
↓
method call
↓
nested method
↓
return
↓
continue
↓
print
```

------------------------------------------------------------------------

# 73. Highest-Priority Topics to Practice

If exam time is limited, prioritize these:

1.  ⭐ `self.x` vs `A.x`
2.  ⭐ Class variables / static counters
3.  ⭐ Constructors
4.  ⭐ Inheritance
5.  ⭐ `super()`
6.  ⭐ Method overriding
7.  ⭐ Nested method calls
8.  ⭐ Output tracing
9.  ⭐ `*args`
10. ⭐ Object/list aliasing
11. ⭐ `__str__()`
12. ⭐ `__add__()`
13. ⭐ Optional/default arguments
14. ⭐ Scope: local vs instance
15. ⭐ Dictionaries and shared class data

------------------------------------------------------------------------

# 74. Final Exam Mindset

When you see a complicated CSE111 code-tracing question, **do not try to
understand the whole code at once**.

Use this formula:

``` text
1. Find class variables
2. Find instance variables
3. Create objects
4. Trace constructors
5. Track self.x / self.y / self.sum
6. Track Class.x / Class.temp
7. Trace one method at a time
8. Stop at nested calls
9. Finish nested call
10. Continue
11. Record print order
12. Check return values
```

### The most important distinction

``` text
x      → local
self.x → object
A.x    → class
```

If you can keep these three separate, most of the Lab 12 tracing
problems become much easier.
