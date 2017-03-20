# The Official Droids On Roids Swift Style Guide

## Introduction

Our style is based on Ray Wenderlich Style Guide with some changes of ours. Here you can get link for Ray Wenderlich style:

* [Ray Wenderlich Swift Style Guide](https://github.com/raywenderlich/swift-style-guide)

## Table of Contents

* [Naming](#naming)
    * [Type Inferred Context](#type-inferred-context)
* [Code Organization](#code-organization)
    * [Unused Code](#unused-code)
    * [Minimal Imports](#minimal-imports)
    * [Spacing](#spacing)
* [Classes and Structures](#classes-and-structures)
    * [App Delegate](#app-delegate)
    * [Singleton](#singleton)
    * [Use of Self](#use-of-self)
    * [Protocol Conformance](#protocol-conformance)
    * [Computed Properties](#computed-properties)
* [Function Declarations](#function-declarations)
* [Closure Expressions](#closure-expressions)
* [Types](#types)
    * [Constants](#constants)
    * [Optionals](#optionals)
    * [Struct Initializers](#struct-initializers)
    * [Type Inference](#type-inference)
    * [Collections](#collections)
    * [Syntactic Sugar](#syntactic-sugar)
* [Control Flow](#control-flow)
* [Statements](#statements)
* [Semicolons](#semicolons)
* [Language](#language)
* [Copyright Statement](#copyright-statement)

## Naming

Use descriptive names, prioritizing clarity over brevity.

* Use `UpperCamelCase` for types and protocols
* Use `camelCase` for everything else

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

**Preffered:**
```swift
var floatNumber: Float = 1.0
```
**Not Preffered:**
```swift
var floatNumber: Float = 1
```

For functions and init methods, prefer not omitting argument labels for all parameters unless the context is very clear. Include argument labels before parameters if it makes function calls more readable.

```swift
func date(from string: String) -> Date
func convertPoint(atColumn column: Int, row: Int) -> CGPoint
func timedAction(delay delay: TimeInterval, perform action: SKAction) -> SKAction!

// would be called like this:
date(from: "2014-03-14")
convertPoint(atColumn: 42, row: 13)
timedAction(delay: 1.0, perform: someOtherAction)
```

### Type Inferred Context

Use compiler inferred context to write shorter, clear code.

**Preferred:**
```swift
let view = UIView(frame: .zero)
view.backgroundColor = .red
```

**Not Preferred:**
```swift
let view = UIView(frame: CGRecrt.zero)
view.backgroundColor = UIColor.red
```

## Code Organization

### Unused Code

Unused (dead) code, including Xcode template code and placeholder comments should be removed. An exception is when your tutorial or book instructs the user to use the commented code.

### Minimal Imports

Keep imports minimal. For example, don't import UIKit when importing Foundation will suffice.

### Spacing

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
class Example {

    @IBOutlet weak var tableView: UITableView!
    @IBOutlet weak var collectionView: UICollectionView!

    weak var someDelegate: SomeDelegate?
    weak var anotherDelegate: AnotherDelegate?

    var someArray: [String]?
    var counter = 0
}
```
* Use exactly one new line at the beginning of a class or struct to add clarity. No new lines at the end.

```swift
class Example {

    var someArray: [Float]?
}
```

## Classes and Structures

### Which one to use?

Remember, structs have [value semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_144). Use structs for things that do not have an identity. An array that contains `[a, b, c]` is really the same as another array that contains `[a, b, c]` and they are completely interchangeable. It doesn't matter whether you use the first array or the second, because they represent the exact same thing. That's why arrays are structs.

Classes have [reference semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_145). Use classes for things that do have an identity or a specific life cycle. You would model a person as a class because two person objects are two different things. Just because two people have the same name and birthdate, doesn't mean they are the same person. But the person's birthdate would be a struct because a date of 3 March 1950 is the same as any other date object for 3 March 1950. The date itself doesn't have an identity.

### Example definition

Here's an example of a well-styled class definition:

```swift
class Circle: Shape {

    var x: Int, y: Int
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

    override func area() -> Double {
        return Double.pi * radius * radius
    }
}

extension Circle: CustomStringConvertible {
    var description: String {
        return "center = \(centerString) area = \(area())"
    }
    private var centerString: String {
        return "(\(x),\(y))"
    }
}
```

The example above demonstrates the following style guidelines:

 + Specify types for properties, variables, constants, argument declarations and other statements with a space after the colon but not before, e.g. `x: Int`, and `Circle: Shape`.
 + Use computed properties instead of functions when it only returns an instance of class or object and doesn't modify existing instances or states.
 + Define multiple variables and structures on a single line if they share a common purpose / context.
 + Indent getter and setter definitions and property observers.
 + Don't add modifiers such as `internal` when they're already the default. Similarly, don't repeat the access modifier when overriding a method.
 + Organize extra functionality (e.g. printing) in extensions.
 + Hide non-shared, implementation details such as `centerString` inside the extension using private access control.
 + Don't bloat view controller's life-cycle methods. Encapsulate code to new setup methods to achieve code clarity.

### App Delegate

Limit your code in App Delegate to an absolute minimum.

### Singleton

Take advantage of Swift's modern syntax and create singletons using static class variables.

***Remember***: Use singletons only when it's really necessary.

**Example**
```swift
class Users {

    static let shared = Users()
}
```

### Use of Self

For conciseness, avoid using `self` since Swift does not require it to access an object's properties or invoke its methods.

Use `self` only when required by the compiler (in `@escaping` closures, or in initializers to disambiguate properties from arguments). In other words, if it compiles without `self` then omit it.

```swift
class BoardLocation {

    let row: Int, column: Int

    init(row: Int, column: Int) {
        self.row = row
        self.column = column

        _ = { [unowned self] in
            print(self.row)
        }
    }
}
```

### Protocol Conformance

When adding protocol conformance to a class, prefer adding a separate class extension for the protocol methods. This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a class with its associated methods.

Avoid using `// MARK: -` comment to describe extensions. Extension's name should be self-explanatory.

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
class ViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
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

## Function Declarations

Keep function declarations in one line including the opening brace:

```swift
func reticulate(splines: [Double]) -> Bool {
    // reticulate code goes here
}
```

For functions that take any kind of mathematical parameters or geometric variables, all those parameters should have explicit, non-omitted names:

```swift
func area(x: Double, y: Double) -> Double {
    // code
}
```

## Closure Expressions

* Use trailing closure syntax only if there's a single closure expression parameter at the end of the argument list. Give the closure parameters descriptive names.
* Use `[unowned self]` in closures if you're sure that `self` will **NEVER** be nil.
* Use `[weak self]` in closures if you're sure that `self` **COULD** be nil.

**HINT:** For reference and further understanding on the subject please check the [Automatic Reference Counting](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AutomaticReferenceCounting.html) explanation.

**Preferred:**
```swift
UIView.animate(withDuration: 1.0) {
    self.myView.alpha = 0.0
}

UIView.animate(withDuration: 1.0, animations: {
    self.myView.alpha = 0.0
}, completion: { finished in
    self.myView.removeFromSuperview()
})
```

**Not Preferred:**
```swift
UIView.animate(withDuration: 1.0, animations: {
    self.myView.alpha = 0.0
})

UIView.animate(withDuration: 1.0, animations: {
    self.myView.alpha = 0.0
}) { f in
    self.myView.removeFromSuperview()
}
```

* For single-expression closures where the context is clear, use implicit returns:

```swift
attendees.sort { a, b in a > b }
```

* Remove not needed `() -> Void` statements in closures. Exception to that rule is when Apple requires the explicit type from us.

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

Use `CGFloat` if it makes the code more succinct by avoiding too many conversions.

### Constants

Constants are defined using the `let` keyword, and variables with the `var` keyword. Always use `let` instead of `var` if the value of the variable will not change.

**Tip:** A good technique is to define everything using `let` and only change it to `var` if the compiler complains!

You can define constants on a type rather than on an instance of that type using type properties. To declare a type property as a constant simply use static let. Type properties declared in this way are generally preferred over global constants because they are easier to distinguish from instance properties. Example:

**Preffered:**
```swift
class Guideline {

    private enum Constants {
        static let height = 100.0
        static let title = "Title"
    }
}
```

**Note:** The advantage of using a case-less enumeration is that it can't accidentally be instantiated and works as a pure namespace.

**Not Preffered:**
```swift
class Guideline {

    let height = 100.0
    let title = "Title"
}
```

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

**Preferred:**
```swift
var subview: UIView?
var volume: Double?

// later on...
if let subview = subview, let volume = volume {
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

Prefer the struct-scope constants `CGRect.infinite`, `CGRect.null`, etc. over global constants `CGRectInfinite`, `CGRectNull`, etc. For existing variables, you can use the shorter `.zero`.

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

### Collections

Usage of methods such as `map`, `flatMap`, `filter`, `enumerated`, `first`, `last`, `prefix`, `suffix` is strongly encouraged.

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

* Use convenient Swift Core Geometry methods such as `someFrame.width` or `someframe.midX` to access view's properties.
* Use `nil` only in method calls or assignments. Compare to `nil` in conditional statements, if it increases the readability of your code.

## Control Flow

Prefer the `for-in` style of `for` loop over the `while-condition-increment` style.

**Preffered:**
```swift
for _ in 0..<3 {
    print("Hello three times")
}
```

**Not Preffered:**
```swift
var i = 0
while i < 3 {
    print("Hello three times")
    i += 1
}
```

### Switch
* Use `()` notation instead of `break` in default switch cases when there is no other instruction contained in it.

**Example**
```swift
switch mobileOS {
case .iOS:
    print("Yes, this is iOS")
default: ()
}
```

## Statements

* Use `guard` statement to exit the method execution early when condition ***is not met***.
* Keep the amount of code in a guard's `else` statement to a minimum.

**Example**
```swift
guard let string = string, string != "someString" else {  
    fatalError("something went wrong")
}
```
* Keep `guard` statements comma-separated.

**Example**
```swift
var first: Int?
var second: Int?

func letGuard() {
    guard let first = first, let second = second else { return }
    print(first)
    print(second)
}
```

## Semicolons

Swift does not require a semicolon after each statement in your code. They are only required if you wish to combine multiple statements on a single line.

Do not write multiple statements on a single line separated with semicolons.

**Preferred:**
```swift
let swift = "not a scripting language"
```

**Not Preferred:**
```swift
let swift = "not a scripting language";
```

**NOTE**: Swift is very different to JavaScript, where omitting semicolons is [generally considered unsafe](http://stackoverflow.com/questions/444080/do-you-recommend-using-semicolons-after-every-statement-in-javascript).

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
     * Copyright (c) 2017 Droids On Roids LLC
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
