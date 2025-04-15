# Swift Distilled

This guide serves as a reference of essentials for the subset of Swift necessary for coding challenge problems. Other resources tend to be too iOS focused or all encompassing.

Learning iOS development, SwiftUI, or server side Swift is beyond the scope of this repo, and I would point you toward https://www.swift.org/getting-started/ for some beginner tutorials or https://docs.swift.org for a thorough language reference.

## Why Swift?

Swift is a modern compiled language with pleasant syntax, comprehensible semantics, and powerful flexibility. To give a working definition of "modern", suppose it is a language designed with high level abstractions, nullable types, memory safety, static type checking, higher-order functions, and concurrency where the language encourages good programming practices. Swift recently modernized its concurrency model in Swift 5.5 to use `async/await`.

It is [open source](https://github.com/swiftlang/swift) and cross-platform with tooling for use outside of Xcode. There is a version manager called [swiftly](https://www.swift.org/blog/introducing-swiftly_10/), the [Swift Package Manager](https://www.swift.org/documentation/package-manager/), an [official VS Code Swift extension](https://www.swift.org/documentation/articles/getting-started-with-vscode-swift.html), and configuration [guides](https://www.swift.org/tools/) for Emacs and Neovim. As of Swift 6.0, Windows and Linux users get the same `Foundation` package as MacOS.

"Swift is intended as a replacement for C-based languages (C, C++, and Objective-C)." It has full native C interoperability without FFI, and [C++ interoperability](https://www.swift.org/documentation/cxx-interop/) as an evolving feature since Swift 5.9. Swift is being used in several domains of software such as embedded systems, systems programming, [server side development](https://developer.apple.com/videos/play/wwdc2024/10216/), and of course iOS applications. Swift [embedded](https://github.com/swiftlang/swift-evolution/blob/main/visions/embedded-swift.md), a subset of Swift without a runtime, powers the [Secure Enclave](https://support.apple.com/guide/security/secure-enclave-sec59b0b31ff/web) in Apple devices.

## Using Swift in Coding Challenges

For those that use and enjoy Swift, know that Swift is more than up to the task. LeetCode is using Swift 6.x with the `Foundation`, `Collections`, `Algorithms`, and `Numerics` packages available for problem solving. You will find Swift as a language option on most popular coding challenge platforms, but of course verify its language and package availability ahead of time.

> [!Important]
> There are four things to keep in mind when using Swift. Check each relevant section of this guide for further information and examples.
>
> 1. [Strings](#strings)
>    * They are encoded in Unicode and integer subscripting is not available
>    * Easy workaround is to convert strings with `Array(nameOfString)`
> 2. [Stacks](#stacks) / [Queues](#queues)
>    * Array can be used as a stack for O(1) insert and removal
>    * Removing elements from the front of an array is O(n)
>    * If the time complexity of queue operations matter, use `deque` from the `Collections` package
> 3. [Heaps](#heaps) / Priority Queue
>    * Requires the Apple `Collections` package which is not apart of the standard library
> 4. [Optionals](#optionals)
>    * Values that can be `nil` need extra handling
>    * Most prevalent in graph or tree problems

## Table of Contents

- [Variables](#variables)
- [Operators](#operators)
- [Math](#math)
- [Control Flow](#control-flow)
  + [Conditionals](#conditionals)
  + [Loops](#loops)
- [Strings](#strings)
  - [Subscripting](#subscripting)
  - [Substrings](#substrings)
- [Tuples](#tuples)
- [Functions](#functions)
- [Collections](#collections)
  - [Arrays](#arrays)
  - [2D Arrays](#2d-arrays)
  - [Stacks](#stacks)
  - [Queues](#queues)
  - [Sets](#sets)
  - [Dictionaries](#dictionaries)
- [Deque](#deque)
- [Heaps](#heaps)
- [Optionals](#optionals)
- [Classes](#classes)
- [Closures](#closures)

  

## Variables

Mutable variables are declared with the  `var` keyword.

```swift
var smallPrime: Int = 31
```

Immutable constants are declared with the  `let` keyword. 

```swift
let eulers: Float = 2.71828 // Float is 32bit
```

Swift will default to `Double` when using type inference on a decimal value. Here is a `Double` with an explicit type annotation.

```swift
let pi: Double = 3.14159265358979323846 // Double is 64bit
```

The Swift language is statically typed and type safe.

```swift
let bestLanguage: String = "Swift"
var isThatAFact: Bool = true
```

Instead of the type annotations in the examples above (`: Int`, `: String`, etc.), use Swift's type inference for DSA problems.

```swift
var willCompile = "Yes"
```

You can declare and initialize multiple values on the same line.

```swift
var left = 0, right = 1, count = 0
```

To increment or decrement you can use the `+=`  or `-=` operators, or write it out the long way if you prefer.

```swift
left += 1
right += 1
right = right + 1
// note: left++ syntax is invalid in Swift
```

```swift
let decimalInteger = 15
let binaryInteger = 0b1111      // 15 in binary notation
let octalInteger = 0o17         // 15 in octal notation
let hexadecimalInteger = 0xf    // 15 in hexadecimal notation
```

## Operators

Arithmetic.

```swift
1 + 1       // is 2
5 - 1       // is 4
3 * 4       // is 12
7 / 2       // is 3
7 / 2.0     // is 3.5
```

In Swift the `%` operator is actually a remainder operator and doesn't perform true modulo operations.

```swift
9 % 4       // is 1
-9 % 4      // is -1 (not 3 like you would expect)
```

To get traditional modulo behavior, when `a % n` is negative, add `n` to the result.

```swift
func mod(_ a: Int, _ n: Int) -> Int {
    precondition(n > 0, "modulus must be positive")
    let r = a % n
    return r >= 0 ? r : r + n
}
```

Comparison operators.

```swift
==  // equal
!=  // not equal
>   // greater than
>=  // greather than or equal to
<   // less than
<=  // less than or equal to
```

Logical operators.

```swift
&&  // logical AND
||  // logical OR
!   // logical NOT
```

Range operators.

```swift
a...b // closed range from a to b inclusive
a..<b // half-open range is from a to b exclusive (does not include b)
```

The ternary is shorthand for `if a then b else c` in the form of `a ? b : c` so the if-block below and the ternary statement are equivalent and both of them output `true`

```swift
let bestLanguage = "Swift"

if bestLanguage == "Swift" {
    print(true)
} else {
    print(false)
}

// ternary
bestLanguage == "Swift" ? print(true) : print(false)

// true
// true
```

The nil-coalescing operator will unwrap `a` if it contains a value, else it will return a default value `b`

```swift
a ?? b               // shorthand for a != nil ? a! : b
stringVariable ?? "" // will return an empty-string if stringVariable is nil 
```



## Math

Note that `Foundation` import is required for `floor` and `ceil` functions.

```swift
import Foundation

var someNumber = 7.3
floor(someNumber) // 7.0

var anotherNumber = 10.2
ceil(anotherNumber) // 11.0
```

For square root use `variable.squareRoot()` with `Float` or `Double` types

```swift
var someNumber = 121.0
someNumber.squareRoot() // 11.0
```

For powers you can use the exponentiation operator `**` 

```swift
3 ** 3 // 27 
```

Or import `Foundation` and use `pow(_:_)`

```swift
import Foundation

let base = 3.0
let exponent = 3.0
pow(base, exponent) // 27
```

There is min/max so that you don't have to type things like `if a < b { min = a } else { min = b }`...

```swift
let a = 3
let b = 27

min(a, b) // 3
max(a, b) // 27
```

Integers can overflow in Swift, unlike in Python, so be careful.

```swift
Int.min // -9223372036854775808
Int.max // 9223372036854775807
```

Use `&+` `&-` and `&*` to wrap around and avoid an overflow.

```swift
var bigNumber = Int.max

bigNumber + 1  // error: overflow
bigNumber &+ 1 // wraps around to Int.min
```

Logarithms.

```swift
import Foundation

// natural log
var someNumber = 8.0
log(someNumber) // 2.079

// log base 2
log2(someNumber) // 3

// log base 10 requires change-of-base formula
log(someNumber) / log(10.0) // 0.903..
```

## Control Flow

### Conditionals

`if` statements in Swift are similar to other languages.

```swift
let temperature = 39

if temperature > 45 {
    print("Summer Tires")
} else if temperature > 32 {
    print("All-Season Tires")
} else {
    print("Winter Tires")
}
```

### Loops

For-loops in Swift use Range Operators. By default the lower bound is inclusive. Closed range is `...` and half-open range is `..<`

```swift
let closedRange = 0...2   // closed range is from 0 to 2
let halfOpenRange = 0..<2 // half-open range is from 0 to 1

closedRange.contains(2)   // true
halfOpenRange.contains(2) // false
```

Here are Range Operators being used in For-in loops:

```swift
for i in 0...2 {
    print(i)
}
// 0
// 1
// 2

for i in 0..<2 {
    print(i)
}
// 0
// 1
```

`stride(from:to:by:)` in Swift is equivalent to Python's `range(start:stop:step)`

```swift
let start = 2
let finish = 10

for i in stride(from: start, to: finish, by: 2) {
    print(i)
}
// 2
// 4
// 6
// 8
```

`while` loops evaluate the condition at the beginning of each iteration.

```swift
while <#condition#> {
   <#statements#>
}

var countdown = 10

while countdown > 0 {
    print(countdown)
    countdown -= 1
}
print("Happy New Year!")
```

`repeat-while` is analogous to a `do-while` in other languages, where the condition is evaluated at the end of each iteration.

```swift
repeat {
   <#statements#>
} while <#condition#>

let password = "New England clam chowder"
var attempt: String

repeat {
    print("What's the password?")
    attempt = readLine() ?? ""
    
    if attempt != password {
        print("Try again")
  }
} while attempt != password

print("Is that the red or the white?")
```

## Strings

### Subscripting

Strings in Swift are encoded in Unicode such that each `Character` is an extended grapheme cluster. Since a single Unicode character can be represented by one or more bytes (i.e. emojis or accented characters), you cannot subscript a string using an integer index.

https://docs.swift.org/swift-book/documentation/the-swift-programming-language/stringsandcharacters

```swift
var apple = "Macintosh"
print(apple[2]) // error
```

There is a way to subscript strings with the `index` method using `(before: after: offsetBy:)`, but it is simpler to convert it to an array for DSA problems and save the `index` method for working with actual Unicode.

```swift
var raspberry = "Pi"
var charArray = Array(raspberry)
print(charArray[0]) // "P"
```

### Substrings

You can use range operators to create substrings where `...` can get you everything until the beginning or end of the array and the half-open range operator `..<` will exclude the upper bound.

```swift
var greeting = "Happy New Year!"
var greetingArray = Array(greeting)
let happy = greetingArray[...5]
let new = greetingArray[6..<9]
let year = greetingArray[10...]
print(String(happy)) // "Happy"
print(String(new))   // "New"
print(String(year))  // "Year!"
```

Strings can be concatenated with the `+=` compound operator or you can use the `joined` method to concatenate multiple strings together.

```swift
// concatenating with +=
var halfProgram = "Hello "
halfProgram += "World!"

var wholeProgram = halfProgram
print(wholeProgram) // "Hello World!"

// joined method
var stringList: [String] = ["Red", "White", "Blue"]
let murica = stringList.joined(separator: " ")
print(murica) // "Red White Blue"
```

Here are some examples of common string and integer conversions.

```swift
// converting numeric strings to integers
let goodPrime: Int = Int("31") ?? 0
let meaningOfLife: Int = Int("42") ?? 0
let sheldon: Int = goodPrime + meaningOfLife
print(sheldon) // 73

// converting integers to numeric strings
let deja = 123
let vu = 123
print("\(deja)\(vu)") // "123123"

// getting the ASCII value of a character
let alphaOne = "A"
let alphaTwo = "B"

if let asciiValue1 = alphaOne.utf8.first {
    print(asciiValue1) // 65
}

if let asciiValue2 = alphaTwo.utf8.first {
    print(asciiValue2) // 66
}
```

## Tuples

Since Swift does not support multiple return values, tuples are probably most useful in those situations. Tuples can be created with any permutation of types like (Int, Bool, Int, String), or whatever you want.

```swift
// creating tuples
let httpStatus = (503, "Service Unavailable")

// they can be named
let httpStatusNamed = (statusCode: 503, statusMessage: "Service Unavailable")

// or decomposed into variables
let (statusCode, statusMessage) = httpStatus
print("Response status code returned: \(statusCode)") // 503

// or accessed by their integer index
print("The status message was: \(httpStatus.1)") // "Service Unavailable"

// and you can use a '_' to discard values you don't want
let (code, _) = httpStatus
```

## Functions

Functions don't need parameters or return values.

```swift
func basic() {
    print("Much basic")
}
basic() // "Much basic"
```

Swift uses argument labels which are `greeting` and `name` in the following snippet.

```swift
func greeting(greeting: String, name: String) -> String {
    return "\(greeting) \(name)!"
}
print(greeting(greeting: "Bonjour", name: "Bob")) // "Bonjour Bob!"
```

You probably want to omit argument labels for simpler function calling.

```swift
func greeting(_ greeting: String, _ name: String) -> String {
    return "\(greeting) \(name)!"
}
print(greeting("Ciao", "Alice")) // "Ciao Alice!"
```

Tuples are how Swift functions can support multiple return values.

```swift
func minMax(_ array: [Int]) -> (min: Int, max: Int) {
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}

let arr: [Int] = [1, 2, 3, 4, 5]
let (min, max) = minMax(arr)
print("The minimum value in \(arr) is \(min) and the max is \(max).")
```

In Swift, parameters use value semantics by default. To simulate pass-by-reference behavior for value types, you can use the `inout` keyword. This allows a function to modify the original variable by copying its value into the function, modifying it, and then copying the modified value back to the original variable when the function returns. This is useful when you need to modify a data structure in place without returning a new value.

```swift
/// removeElement removes all elements in the original array that are equivalent to `val` and returns the total number of elements removed
func removeElement(_ nums: inout [Int], _ val: Int) -> Int {
    var i = nums.count - 1
    while i >= 0 {
        if nums[i] == val {
            nums.remove(at: i)
        }
        i -= 1
    }    
    return nums.count
}

// note that the list is mutable
var someList = [3, 2, 2, 3]

// & is needed when modifying the original variable
print(removeElement(&someList, 2)) // [3, 3]
```

> [!Tip] 
> Swift allows nested functions which are handy for helper/utility/accumulator functions.

## Collections

### Arrays

Arrays are Swift's lists and can be stored in mutable variables or immutable constants.

```swift
let immutableList: [Int] = []  // creates an immutable empty Int list
var mutableList: [String] = [] // creates a mutable empty String list

print(immutableList) // []
print(mutableList)   // []
```

Arrays can be initialized with default values or you can create an array literal.

```swift
// default values
var tripleZero = Array(repeating: 0, count: 3)
print(tripleZero) // [0, 0, 0]

// array literal
let numList: Int = [1, 2, 3]
print(numList) // [1, 2, 3]
```

Here are some core array operations:

```swift
// append
var arr = [Int]()
arr.append(1)
arr.append(2)
arr.append(3)
print(arr) // [1, 2, 3]

// remove
arr.remove(at: 2)
print(arr) // [1, 2]

// insert
arr.insert(4, at: 1)
print(arr) // [1, 4, 2]

// isEmpty()
print(arr.isEmpty()) // false

// count
print(arr.count) // 3
```

The largest index in an array is `array.count - 1` because arrays are indexed starting from zero.

```swift
var arr = [Int]()
print(arr.count) // 0

for i in 1...5 {
    arr.append(i)
}
print(arr.count) // 5
print(arr[0])    // 1
print(arr[4])    // 5
print(arr[5])    // error: out of range
```

Arrays can be sorted in place where you mutate the original data structure using the `sort` method.

```swift
var arr: [Int] = [5, 1, 3, 4, 2]

arr.sort()
print(arr) // [1, 2, 3, 4, 5]
```

You can also return a new array without mutating the original by using the `sorted` method.

```swift
var arr: [Int] = [5, 1, 3, 4, 2]

// sort in ascending order
let ascendingArr = arr.sorted()
print(ascendingArr)  // [1, 2, 3, 4, 5]
print(arr)           // [5, 1, 3, 4, 2]

// sort in descending order
let descendingArr = arr.sorted(by: >) // (by:) works with sort() method as well
print(descendingArr) // [5, 4, 3, 2, 1]
print(arr)           // [5, 1, 3, 4, 2]
```

### 2D Arrays

You can initialize an 2D array literal as follows.

```swift
var matrix: [[Int]] = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print(matrix) // prints the matrix no loop required
```

To initialize a 2D array with 0's:

```swift
let rows = 2
let columns = 3
var matrix = [[Int]](repeating: [Int](repeating: 0, count: columns), count: rows)
print(matrix) // [[0, 0, 0], [0, 0, 0]]
```

Here is how you access and modify elements.

```swift
var matrix: [[Int]] = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

// access a value
let four = matrix[1][0] // 4

// set a value
matrix[1][1] = 4
matrix[1][2] = 4
print(matrix) // [[1, 2, 3], [4, 4, 4], [7, 8, 9]]

// add a row
matrix.append([0, 0, 0])
print(matrix) // [[1, 2, 3], [4, 4, 4], [7, 8, 9], [0, 0, 0]]

// iterate over 2D array
for i in 0..<matrix.count {
    for j in 0..<matrix[i].count {
        print(matrix[i][j])
    }
}
```

### Stacks

You can use an array as a stack with `O(1)` insert and `O(1)` removal.

```swift
var stack: [Int] = []

// push values onto the stack
stack.append(1)
stack.append(2)
stack.append(3)
print(stack)                  // [1, 2, 3]

// peek
print(stack.last ?? 0)        // 3

// pop
print(stack.popLast() ?? 0)   // 3
print(stack.last ?? 0)        // 2
print(stack.isEmpty)          // false
print(stack)                  // [1, 2]
```

### Queues

For queues you can also use an array, but be aware of the time complexity difference. Dequeueing from the front of the queue is an `O(n)` operation because all elements have to shift to the left.

```swift
var queue: [Int] = []

// enqueue
queue.append(1)
queue.append(2)
queue.append(3)
print(queue)

// peek
print(queue.first ?? 0)      // 1
print(queue)                 // [1, 2, 3]

// dequeue
print(queue.removeFirst())   // 1
print(queue)                 // [2, 3]
```

### Sets

```swift
var musicGenres: Set<String> = ["Classical", "Jazz", "Electronic"]
musicGenres.count // 3

// insert
let _ = musicGenres.insert("Jazz") // false

// clear
musicGenres.removeAll()
musicGenres.count // 0

musicGenres = ["Classical", "Jazz", "Electronic"]
musicGenres = []
musicGenres.count // 0
```

Sets in Swift are unordered, so if you need ordering you can use the `sorted` method.

```swift
var musicGenres: Set<String> = ["Classical", "Jazz", "Electronic"]
for genre in musicGenres {
    print(genre) // not guaranteed to print in order
}

for genre in musicGenres.sorted() {
    print(genre) // prints in ascending order
}
```

Here are some core set operations:

```swift
let oddDigits: Set = [1, 3, 5, 7, 9]
let evenDigits: Set = [0, 2, 4, 6, 8]
let singleDigitPrimeNumbers: Set = [2, 3, 5, 7]

let universalSet = oddDigits.union(evenDigits) // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

// compliment
universalSet.subtracting(singleDigitPrimeNumbers) // [0, 1, 4, 6, 8, 9]

// intersection
singleDigitPrimeNumbers.intersection(oddDigits).sorted() // [3, 5, 7]

// difference
oddDigits.subtracting(singleDigitPrimeNumbers).sorted() // [1, 9]

// symmetric difference
oddDigits.symmetricDifference(singleDigitPrimeNumbers).sorted() // [1, 2, 9]
```

In additional to fundamental set operations, you can check set equality with `==` and compute subset, superset, and disjoint as follows:

```swift
let myHouseAnimals: Set = ["Cat", "Dog"]
let houseAnimals: Set = ["Dog", "Cat"]
let farmAnimals: Set = ["Cow", "Chicken", "Sheep", "Dog", "Cat"]
let cityAnimals: Set = ["Bird", "Mouse"]

myHouseAnimals == houseAnimals // true
houseAnimals.isSubset(of: farmAnimals)    // true
farmAnimals.isSuperset(of: houseAnimals)  // true
farmAnimals.isDisjoint(with: cityAnimals) // true
```

Here's how you can convert a set to a list or a list to a set:

```swift
var cityAnimals: Set = ["Bird", "Mouse"]

// convert set to list and insert elements
var cityAnimalsList = Array(cityAnimals) // ["Bird, "Mouse"]
cityAnimalsList.append("Bird")
cityAnimalsList.append("Mouse")
cityAnimalsList.append("Rat")
print(cityAnimalsList) // ["Bird", "Mouse", "Bird", "Mouse", "Rat"]

// convert list to set
cityAnimals = Set(cityAnimalsList) // ["Bird, "Mouse", "Rat"]
```

### Dictionaries

```swift
// declare empty dictionary
var newDict: [String: Int] = [:]

// dictionary literal
var mentions = [
    "Aragorn": 952,
    "Legolas": 340,
    "Gandalf": 1168,
    "Gimli": 390,
    "Frodo": 1983,
    "Sam": 714,
    "Merry": 634,
    "Pippin": 734,
]

mentions.count // 8

// insert (key:value) pair
mentions["Galadriel"] = 84

// get value
let whySoLow = mentions["Gimli"]

// set value
mentions["Gimli"] = 9999
```

Note that dictionaries do not have a defined ordering so use the `sorted` method if you need to iterate in a specific order.

```swift
// using the mentions dictionary from the previous snippet...

// printing pairs
for (name, mentions) in mentions {
    print("\(name): \(mentions)")
}

// iterate over keys
for name in mentions.keys {
    print(name)
}

// iterate over values in sorted order
for mentions in mentions.values.sorted() {
    print(mentions)
}
```

## Deque

The `deque`, or double ended queue, data structure can be imported from the `Collections` package. This is the `apple/swift-collections` package on the Swift Package Index and although officially provided by Apple, it is not apart of the standard library in Swift. LeetCode includes this package as well as the `Algorithms` and `Numerics` packages when solving problems on their site. If you are using a platform that does not include the `Collections` package, keep the time complexity in mind of using an array.

```swift
import Collections

var deque: Deque<Int> = [1, 2, 3]
print(deque.prepend(0)) // [0, 1, 2, 3]
print(deque.append(0))  // [0, 1, 2, 3, 0]
```

## Heaps

The `Collections` package is necessary to use the `heap` structure in Swift. To get the element with the lowest priority use the `min` method. To get the element with the highest priority use the `max` method. To remove and return those elements, use `popMin` or `popMax` methods.

```swift
import Collections

var heap: Heap<Int> = [3, 4, 1, 2]

// insert
heap.insert(0)

// returns element with lowest priority
print(heap.min ?? 0)
// 0

// remove and return element with highest priority
print(heap.popMax() ?? 0)
// 4

// returns element with highest priority
print(heap.max ?? 0)
// 3

// remove and return element with lowest priority
print(heap.popMin() ?? 0)
// 0

print(heap.unordered)
// [1, 2, 3]
```

## Optionals

Optionals are types that can either contain a value, or `nil`, the absence of a value. The purpose of having this type is for null safety and to prevent Null Pointer Exceptions. The term `nil` comes from Objective-C and is conceptually the same as `null`. Since Swift is fully interoperable with Objective-C and many Apple platform APIs are written in Objective-C, the choice for `nil` in the Swift language makes sense.

Appending a `?` to a type in Swift makes it an `optional` type

```swift
var userName: String? = "Steve"
```

If we try and use print that variable, we get the following in our console:

```swift
print(userName) // Optional("Steve")
```

Swift's optionals can be thought of as an `enum` type that either contain `none` or `some` which needs to be unwrapped.

```swift
enum Optional<Wrapped> {
    case none           // represents nil, absence of a value
    case some(Wrapped)  // contains a value of type Wrapped
}
```

> [!Warning]
> If you want to unwrap an `optional` you can use the `!` operator to force unwrap, but be careful, if you accidentally unwrap an `optional` that contains `nil` your program will crash.

```swift
var userName: String? = nil
print(userName!) // error
```

Instead use a `nil-coalescing` operator `??` to give a default value in case the `optional` is `nil` or an `if let` to prevent an error

```swift
var userName: String? = nil
print(userName ?? "") // ""

if let name = userName {
    print(name) // does not execute
}
```

## Classes

Let's build a linked list class to see a more practical example of `optional` in use because `nil` is used in common linked list implementations to indicate where a list ends.

```swift
public class ListNode {
    public var val: Int
    public var next: ListNode?
    
    public init() {
        self.val = 0
        self.next = nil
    }
    
    public init(_ val: Int) {
        self.val = val
        self.next = nil
    }
    
    public init(_ val: Int, _ next: ListNode?) {
        self.val = val
        self.next = next
    }
}
```

Since `ListNode` is `optional` and can be `nil`, we need to handle things carefully. In the following function, `curr` is of type `ListNode` and is handled with a language feature in Swift known as "Optional Chaining". This means the program will immediately return `nil` and fail gracefully instead of causing a runtime error with `!` (force unwrap).

```swift
func reverseList(_ head: ListNode?) -> ListNode? {
    var prev: ListNode? = nil
    var curr = head

    while curr != nil {
        let next = curr?.next
        curr?.next = prev
        prev = curr
        curr = next
    }
    return prev
}
```

Let's test this out.

```swift
func printList(_ head: ListNode?) {
    var curr = head
    while curr != nil {
        print(curr!.val, terminator: " -> ")
        curr = curr?.next
    }
    print("nil")
}

// create a linked list
let node3 = ListNode(3)
let node2 = ListNode(2, node3)
let node1 = ListNode(1, node2)

printList(node1) // 1 -> 2 -> 3 -> nil

// reverse the list
let reversedHead = reverseList(node1)

printList(reversedHead) // 3 -> 2 -> 1 -> nil
```

## Closures

Closures are similar to lambdas or anonymous functions in other languages. I included it in this guide because I find `map`, `filter`, and `reduce` handy. They are simply blocks of functionality that can be passed around or assigned to variables or constants.

```swift
{ (<#parameters#>) -> <#return type#> in
   <#statements#>
}
```

In its simplest form, we can add functionality to this constant and call it like a function.

```swift
let sayHi = {
    print("Hi!")
}

sayHi() // "Hi!"
```

To add parameters and a return type we need to write them before the `in` keyword, and add any statements after in the closure body.

```swift
let sayHi = { (name: String) -> String in
    "Hi \(name)!"
}
sayHi("Alice") // "Hi Alice!"
```

Swift's closures can be written out in a verbose or terse way. This is a an example from the Swift docs where several closures are equivalent.

```swift
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
    return s1 > s2
})

// equivalent
reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )

// equivalent
reversedNames = names.sorted(by: { s1, s2 in s1 > s2 } )

// equivalent
reversedNames = names.sorted(by: { $0 > $1 } )
```

`map`, `filter`, and `reduce` can save you a lot of time as opposed to writing a for-loop.

```swift
let numbers = [1, 2, 3, 4, 5]
let squares = numbers.map { $0 * $0 }       // [1, 4, 9, 16, 25]
let evens = numbers.filter { $0 % 2 == 0 }  // [2, 4]

// initial value is 0
// $0 is the current accumulator value
// $1 is the current element
let sum = numbers.reduce(0) { $0 + $1 }     // 15
```

Here is an example of function chaining where we combine the `map`, `filter`, and `reduce` functions into a single operation.

```swift
let numbers = [1, 2, 3, 4, 5]
let res = numbers
    .map { $0 * $0 }
    .filter { $0 % 2 == 0 }
    .reduce(0) { $0 + $1 }
print(res) // 20
```

## 
