# The Official Droids on Roids Swift Style Guide.

## Introduction

Our style is based on Ray Wenderlich Style Guide with some changes of ours. Here you can get link for Ray Wenderlich style:

* [Ray Wenderlich Swift Style Guide](https://github.com/raywenderlich/swift-style-guide)


## Table of Contents

* [Naming](#naming)
    * [Enumerations](#enumerations)
    * [Class Prefixes](#class-prefixes)
* [Spacing](#spacing)
* [Comments](#comments)
* [Classes and Structures](#classes-and-structures)
    * [App Delegate](#app-delegate)
    * [Singleton](#singleton)
    * [Use of Self](#use-of-self)
    * [Protocol Conformance](#protocol-conformance)
    * [Computed Properties](#computed-properties)
    * [Extensions](#extensions)
* [Function Declarations](#function-declarations)
* [Closure Expressions](#closure-expressions)
* [Types](#types)
    * [Constants](#constants)
    * [Optionals](#optionals)
    * [Struct Initializers](#struct-initializers)
    * [Type Inference](#type-inference)
    * [Arrays](#arrays)
    * [Syntactic Sugar](#syntactic-sugar)
* [Control Flow](#control-flow)
* [Statements](#statements)
* [Semicolons](#semicolons)
* [Language](#language)
* [Copyright Statement](#copyright-statement)


## Naming

Use descriptive names with camel case for classes, methods, variables, etc. Class names should be capitalized, while method names and variables should start with a lower case letter.

**Preferred:**

```swift
private let maximumWidgetCount = 100

class WidgetContainer {

    var widgetButton: UIButton
    let widgetHeightPercentage = 0.85
}
```

**Not Preferred:**

```swift
let MAX_WIDGET_COUNT = 100

class app_widgetContainer {
    var wBut: UIButton
    let wHeightPct = 0.85
}
```

Types such as `Float`, `CGFloat` or `Double` should always be represented as floating point numbers.

**Preffered**
```swift
var floatNumber: Float = 1.0
```
**Not preffered**
```swift
var floatNumber: Float = 1
```

For functions and init methods, prefer named parameters for all arguments unless the context is very clear. Include external parameter names if it makes function calls more readable.

```swift
func dateFromString(dateString: String) -> NSDate
func convertPointAt(column column: Int, row: Int) -> CGPoint
func timedAction(delay delay: NSTimeInterval, perform action: SKAction) -> SKAction!

// would be called like this:
dateFromString("2014-03-14")
convertPointAt(column: 42, row: 13)
timedAction(delay: 1.0, perform: someOtherAction)
```

For methods, follow the standard Apple convention of referring to the first parameter in the method name:

```swift
class Guideline {

    func combineWithString(incoming: String, options: Dictionary?) { ... }
    func upvoteBy(amount: Int) { ... }
}
```

### Enumerations

Use UpperCamelCase for enumeration values.

Every new case should be declared in a new line. Avoid writing one-line, comma-separated cases.

**Preffered**
```swift
enum Shape {
    case Rectangle
    case Square
    case Triangle
    case Circle
}
```

**Not preffered**
```swift
enum Shape {
  case Rectangle, Square, Triangle, Circle
}
```

### Class Prefixes

Swift types are automatically namespaced by the module that contains them and you should not add a class prefix. If two names from different modules collide you can disambiguate by prefixing the type name with the module name.

```swift
import SomeModule

let myClass = MyModule.UsefulClass()
```


## Spacing

* Indent using 4 spaces rather than tabs.

* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.

* Exception: When using the `guard` keyword with an `else` statement containing only a `return` call, you should keep it in one line.

* Tip: You can re-indent by selecting some code (or ⌘A to select all) and then Control-I (or Editor\Structure\Re-Indent in the menu).

**Preferred:**
```swift
if user.isHappy {
    // Do something
} else {
   // Do something else
}

guard let _ = superview as? UIView else { return }
```

**Not Preferred:**
```swift
if user.isHappy
{
   // Do something
}
else {
   // Do something else
}

if user.isHappy { /* Do something */ }

guard let _ = superview as? UIView else {
   return
}
```

* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but having too many sections in a method often means you should refactor into several methods.

* Use one blank line to separate the variables declarations. Outlets at the top, then the delegate, and then the rest of the variables.

```swift
class ExampleClass {

  @IBOutlet weak var tableView: UITableView!
  @IBOutlet weak var collectionView: UICollectionView!

  weak var someDelegate: SomeDelegate?
  weak var anotherDelegate: AnotherDelegate?

  var someArray: [String]?
  var counter = 0
}
```
* Use exactly one new line at the beggining of a class or struct to add clarity. No new lines at the end.

```swift
class SomeClass {

    var someArray: [Float]?
}
```

## Comments

* When they are needed, use comments to explain **why** a particular piece of code does something. Comments must be kept up-to-date or deleted.

* Avoid block comments inline with code, as the code should be as self-documenting as possible. *Exception: This does not apply to those comments used to generate documentation.*

* Comments should rather say what is the code doing, not how

## Classes and Structures

### Which one to use?

Remember, structs have [value semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_144). Use structs for things that do not have an identity. An array that contains [a, b, c] is really the same as another array that contains [a, b, c] and they are completely interchangeable. It doesn't matter whether you use the first array or the second, because they represent the exact same thing. That's why arrays are structs.

Classes have [reference semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_145). Use classes for things that do have an identity or a specific life cycle. You would model a person as a class because two person objects are two different things. Just because two people have the same name and birthdate, doesn't mean they are the same person. But the person's birthdate would be a struct because a date of 3 March 1950 is the same as any other date object for 3 March 1950. The date itself doesn't have an identity.

Sometimes, things should be structs but need to conform to `AnyObject` or are historically modeled as classes already (`NSDate`, `NSSet`). Try to follow these guidelines as closely as possible.

### Example definition

Here's an example of a well-styled class definition:

```swift
class Circle: Shape {

    private var centerX: Int, centerY: Int
    var radius: Double
    var diameter: Double {
        get {
            return radius * 2.0
        }
        set {
            radius = newValue / 2.0
        }
    }

    init(x: Int, y: Int, radius: Double) {
        self.x = x
        self.y = y
        self.radius = radius
    }

    convenience init(x: Int, y: Int, diameter: Double) {
        self.init(x: x, y: y, radius: diameter / 2.0)
    }

    func describe() -> String {
        return "I am a circle at \(centerString()) with an area of \(computeArea())"
    }

    override func computeArea() -> Double {
        return M_PI * radius * radius
    }

    private func centerString() -> String {
        return "(\(x),\(y))"
    }
}
```

The example above demonstrates the following style guidelines:

 + Specify types for properties, variables, constants, argument declarations and other statements with a space after the colon but not before, e.g. `x: Int`, and `Circle: Shape`.
 + Define multiple variables and structures on a single line if they share a common purpose / context.
 + Indent getter and setter definitions and property observers.
 + Make wise use of modifiers but don't add `internal` in a case when it is already the default. Similarly, don't repeat the access modifier when overriding a method.
 + Don't bloat view controller's life-cycle methods. Encapsulate code to new setup methods to achieve code clarity.


### App Delegate

Limit your code in App Delegate to an absolute minimum.

### Singleton

Take advantage of Swift's modern syntax and create singletons using static class variables.

***Remember***: Use singletons only when it's really neccessary

**Example**
```swift
class Users: NSObject {

    static let sharedInstance = Users()
}
```

### Use of Self

For conciseness, avoid using `self` since Swift does not require it to access an object's properties or invoke its methods.

Use `self` when required to differentiate between property names and arguments in initializers, and when referencing properties in closure expressions (as required by the compiler):

```swift
class BoardLocation {

    let row: Int, column: Int

    init(row: Int, column: Int) {
        self.row = row
        self.column = column

        let closure = {
            print(self.row)
        }
    }
}
```

### Protocol Conformance

When adding protocol conformance to a class, prefer adding a separate class extension for the protocol methods. This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a class with its associated methods.

Avoid using `// MARK: -` comment to describe extensions. Extension's name should be self explanatory.

**Preferred:**
```swift
class MyViewController: UIViewController {
    // class stuff here
}

extension MyViewController: UITableViewDataSource {
    // table view data source methods
}

extension MyViewController: UIScrollViewDelegate {
    // scroll view delegate methods
}
```

**Not Preferred:**
```swift
class MyViewcontroller: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
    // all methods
}
```


### Computed Properties

For conciseness, if a computed property is read-only, omit the get clause. The get clause is required only when a set clause is provided.

**Preferred:**
```swift
var diameter: Double {
    return radius * 2.0
}
```

**Not Preferred:**
```swift
var diameter: Double {
   get {
     return radius * 2.0
   }
}
```

### Extensions

Avoid having too many extensions in a view controller. Create new files for additional extensions in order to increase readability.

## Function Declarations

Keep function declarations in one line including the opening brace:

```swift
func reticulateSplines(spline: [Double]) -> Bool {
    // reticulate code goes here
}
```

For functions that take any kind of methematical parameters or geometric variables, all those parameters should have explicit names:

```swift
func exampleMathFuncWith(x x: Double, y: Double) {
  // code
}
```

## Closure Expressions

* Use trailing closure syntax only if there's a single closure expression parameter at the end of the argument list. Give the closure parameters descriptive names.
* Use `[unowned self]` in closures if you're sure that `self` will **NEVER** be nil
* Use `[weak self]` in closures if you're sure that `self` **COULD** be nil

**HINT:** For reference and further understanding on the subject please check the [Automatic Reference Counting](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AutomaticReferenceCounting.html) explanation.

**Preferred:**
```swift
UIView.animateWithDuration(1.0) {
    self.myView.alpha = 0.0
}

UIView.animateWithDuration(1.0, animations: {
        self.myView.alpha = 0.0
    }, completion: { finished in
        self.myView.removeFromSuperview()
    }
)
```

**Not Preferred:**
```swift
UIView.animateWithDuration(1.0,
    animations: {
      self.myView.alpha = 0.0
})

UIView.animateWithDuration(1.0,
    animations: {
        self.myView.alpha = 0.0
    }) { f in
        self.myView.removeFromSuperview()
}
```

* For single-expression closures where the context is clear, use implicit returns:

```swift
attendeeList.sort { a, b in
    a > b
}
```

* Remove not needed `() -> Void` statements in closures, replace `Void` with `()`. Exception to that rule is when Apple requires the explicit type from us. Otherwise remove them.

**Preffered**
```swift
UIView.animateWithDuration(2.0, animations: {
    // ...
  }) { _ in
    // ...
}
```

** Not preffered**
```swift
UIView.animateWithDuration(0.2, animations: { () -> Void in
    // ...
    }) { (_) -> Void in
    // ...
}
```

## Types

Always use Swift's native types when available. Swift offers bridging to Objective-C so you can still use the full set of methods as needed.

**Preferred:**
```swift
let width = 120.0                                    // Double
let widthString = (width as NSNumber).stringValue    // String
```

**Not Preferred:**
```swift
let width: NSNumber = 120.0                          // NSNumber
let widthString: NSString = width.stringValue        // NSString
```

In Sprite Kit code, use `CGFloat` if it makes the code more succinct by avoiding too many conversions.

### Constants

Constants are defined using the `let` keyword, and variables with the `var` keyword. Always use `let` instead of `var` if the value of the variable will not change.

**Tip:** A good technique is to define everything using `let` and only change it to `var` if the compiler complains!


### Optionals

Declare variables and function return types as optional with `?` where a nil value is acceptable.

Use implicitly unwrapped types declared with `!` only for instance variables that you know will be initialized later before use, such as subviews that will be set up in `viewDidLoad`.

When accessing an optional value, use optional chaining if the value is only accessed once or if there are many optionals in the chain:

```swift
self.textContainer?.textLabel?.setNeedsDisplay()
```

Use optional binding when it's more convenient to unwrap once and perform multiple operations:

```swift
if let textContainer = textContainer {
    // do many things with textContainer
}
```

When naming optional variables and properties, avoid naming them like `optionalString` or `maybeView` since their optional-ness is already in the type declaration.

For optional binding, shadow the original name when appropriate rather than using names like `unwrappedView` or `actualLabel`.

**Preferred:**
```swift
var subview: UIView?
var volume: Double?

// later on...
if let subview = subview, volume = volume {
    // do something with unwrapped subview and volume
}
```

**Not Preferred:**
```swift
var optionalSubview: UIView?
var volume: Double?

if let unwrappedSubview = optionalSubview {
    if let realVolume = volume {
        // do something with unwrappedSubview and realVolume
    }
}
```

### Struct Initializers

Use the native Swift struct initializers rather than the legacy CGGeometry constructors.

**Preferred:**
```swift
let bounds = CGRect(x: 40, y: 20, width: 120, height: 80)
let centerPoint = CGPoint(x: 96, y: 42)
```

**Not Preferred:**
```swift
let bounds = CGRectMake(40, 20, 120, 80)
let centerPoint = CGPointMake(96, 42)
```

Prefer the struct-scope constants `CGRect.infiniteRect`, `CGRect.nullRect`, etc. over global constants `CGRectInfinite`, `CGRectNull`, etc. For existing variables, you can use the shorter `.zeroRect`.

### Type Inference

Prefer compact code and let the compiler infer the type for a constant or variable, unless you need a specific type other than the default such as `CGFloat` or `Int16`.

**Preferred:**
```swift
let message = "Click the button"
let currentBounds = computeViewBounds()
var names = [String]()
let maximumWidth: CGFloat = 106.5
```

**Not Preferred:**
```swift
let message: String = "Click the button"
let currentBounds: CGRect = computeViewBounds()
var names: [String] = []
```

**NOTE**: Following this guideline means picking descriptive names is even more important than before.

### CollectionType

Usage of methods such as `map`, `flatMap`, `filter`, `enumerate`, `first`, `last` is strongly encouraged.

### Syntactic Sugar

* Prefer the shortcut versions of type declarations over the full generics syntax.

**Preferred:**
```swift
var deviceModels = [String]()
var employees = [Int: String]()
var faxNumber: Int?
```

**Not Preferred:**
```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```

* Use Core Geometry methods such as `CGRectGetWidth` or `CGRectGetMidY` to access view's properties.
* Use `nil` only in method calls or assignments. Do not compare to `nil` in conditional statements.

## Control Flow

### For-in loop
* Prefer the `for-in` style of `for` loop over the `for-condition-increment` style.

**Preferred:**
```swift
for _ in 0..<3 {
    print("Hello three times")
}

for (index, person) in attendeeList.enumerate() {
    print("\(person) is at position #\(index)")
}
```

**Not Preferred:**
```swift
for var i = 0; i < 3; i++ {
    print("Hello three times")
}

for var i = 0; i < attendeeList.count; i++ {
    let person = attendeeList[i]
    print("\(person) is at position #\(i)")
}
```

### Switch
* Use `()` notation instead of `break` in default switch cases when there is no other instruction contained in it

**Example**
```swift
switch MobileOS(rawValue: int) {
case .Some(.iOS):
    print("Yes, this is iOS")
default: ()
}
```


## Statements

* Use `guard` statement to exit the method execution early when condition ***is not met***.
* Use `where` and `is` operators when it's neccessary
* Keep the amount of code in a guard's `else` statement to a minimum. Ideally only `return` should resign there.

**Example**
```swift
guard let str = string where str != "someString" else {  
    fatalError("something went wrong")
}
```
* Keep `guard` statements comma-separated and avoid repeating `let` or `var` keyword

**Example**
```swift
var first: Int?
var second: Int?

func letGuard() {
    guard let first = first, second = second else { return }
    print(first)
    print(second)
}
```

## Semicolons

Swift does not require a semicolon after each statement in your code. They are only required if you wish to combine multiple statements on a single line.

Do not write multiple statements on a single line separated with semicolons.

The only exception to this rule is the `for-conditional-increment` construct, which requires semicolons. However, alternative `for-in` constructs should be used where possible.

**Preferred:**
```swift
let swift = "not a scripting language"
```

**Not Preferred:**
```swift
let swift = "not a scripting language";
```

**NOTE**: Swift is very different to JavaScript, where omitting semicolons is [generally considered unsafe](http://stackoverflow.com/questions/444080/do-you-recommend-using-semicolons-after-every-statement-in-javascript)

## Language

Use US English spelling to match Apple's API.

**Preferred:**
```swift
let color = "red"
```

**Not Preferred:**
```swift
let colour = "red"
```

## Copyright Statement

The following copyright statement should be included at the top of every source
file:

    /*
     * Copyright (c) 2015 Droids On Roids LLC
     *
     * Permission is hereby granted, free of charge, to any person obtaining a copy
     * of this software and associated documentation files (the "Software"), to deal
     * in the Software without restriction, including without limitation the rights
     * to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
     * copies of the Software, and to permit persons to whom the Software is
     * furnished to do so, subject to the following conditions:
     *
     * The above copyright notice and this permission notice shall be included in
     * all copies or substantial portions of the Software.
     *
     * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
     * IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
     * FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
     * AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
     * LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
     * OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
     * THE SOFTWARE.
     */
