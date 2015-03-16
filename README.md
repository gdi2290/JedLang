JedLang
=======

JedLang is a crazy lisp-like languge that I made up for fun. It compiles to C.

Lispy Math
----------
```shell
(+ 3 2)
(/ 4 2)
(- 4 (/4 2))
```

Output
------
```shell
(@ (+3 2)) //=> 5
(@ "Hello, world.") //=> Hello, world.
```

Functions
---------

Core functions:    
\+ (X, Y) -> returns sum    
\- (X, Y) -> returns difference    
\* (X, Y) -> returns product    
/  (X, Y) -> divides X and Y    
@  (X) -> prints value of X to console    
^  (X, Y) -> prepends X onto Y, where Y is an array    
_  (X, Y) -> returns member of set Y at index X    
\> (X, Y) -> returns true if X is greater than Y    
<  (X, Y) -> returns true if X is less than Y    
?  (X, Y, Z) -> returns Y if X is true, otherwise returns Z    
|  (X) -> returns X    
.  (X) -> returns length of X, where X is an array or a set    

Define a custom function with def. The def command goes like this:

```shell
(def <name> <arguments> <function body>)
```
Make sure to use X Y or Z for you argument names, they correspond to the first, second and third arguments of a function invocation.    

There is another kind of def that uses some core functions, for now there is reduce (REDC) and array (ARRY). They are used like this:

```shell
(def <name> REDC <iterator>) // takes an array as input
(def <name> ARRY <iterator>) // takes a number of iterations and (X) element to iterate
```
The iterator in REDC can take a lowercase letter to stand in for the element at the current index during iteration. The iterator in ARRY can take uppercase letters to stand for the arguments used to invoke the function. For example:
```shell
(def sum REDC (+ e))
(@ (sum [1,2,3,4])) //=> 10
(def len REDC (+ 1))
(@ (len [1,2,3,4])) //=> 4

(def count ARRY (+ X 1))
(@ (count 4 0)) //=> [1,2,3,4]

(def avg X (/ sum len))
(@ (avg (count 4 0))) //=> 2.5
```
This one appends the sum of an array onto the beginning of the array:
```shell
(def tot X (^ (sum X) X))
(@ (tot [1,2,3,4])) //=> [10,1,2,3,4]
```
Lets tack the average on for good measure:
```shell
(def all X (^ (avg X) (tot X)))
(@(all [1,2,3,4])) //=> [2.5,10,1,2,3,4]
```
Sets
----

Sets are not sets in the mathematical sense, they are just groupings of mixed type values. Sets can have set members:

```shell
(set james {"James", 31, "Developer"})
(set mary {"Mary", 29, "Architect"})
(set john {"John", 25, "Intern"})
(set employees {james, mary, john})

(def fst X (_ 0 X))
(def lst X (_ (- (. X) 1) X))
(def job X Y (@ (lst (_ X Y))))

(@ (_ 0 (fst employees))) //=> "James"
(job (- (. employees) 1) employees) //=> "Intern"
```

Booleans
--------
There are functions that output booleans:
```shell
(@ (> 4 3)) //=> True
(@ (< 1 0)) //=> False
```
And a conditional function that takes a boolean function as its first argument:
```shell
(@ (? (> (avg [1,2,3,4]) 1) 10 0)) //=> 10

(def wow X Y (? (> X Y) (@ "wham") (@ "whoozle")))
(wow 4 3) //=> "wham"
```
The conditional function can return functions, as in the two print functions above, or values like in the implementation of fibonnaci below:

```shell
(def low X (? (> X 0) 1 0))
(def fib X (? (< X 3) (low X) (+ (fib (- X 1)) (fib (- X 2)))))
```

USE
---

Write a JedLang file and give it a .jhe extension. To compile and run use the following pattern:

```shell
node parser.js <path/to/filename>

./<filename>.out
```
