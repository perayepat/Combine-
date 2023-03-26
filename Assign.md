
#Overview 
Combine provides two built-in subscribers, which automatically match the output and failure types of their attached publisher:

 - sink(receiveCompletion:receiveValue:) takes two closures. The first closure executes when it receives Subscribers.Completion, which is an enumeration that indicates whether the publisher finished normally or failed with an error. The second closure executes when it receives an element from the publisher.
 - assign(to: on:) immediately assigns every element it receives to a property of a given object, using a key path to indicate the property.

**Using sink**

``` swift
class MyClass {
    var anInt: Int = 0{
        didSet{
            print("anInt was set to: \(anInt)")
        }
    }
}

var myObject = MyClass()
let myRange = (0...2)
let subscription = myRange.publisher.map{ $0 * 10}
    .sink { value  in
        myObject.anInt = value
    }
```

**Using Assign**

```swift
class MyClass {
    var anInt: Int = 0{
        didSet{
            print("anInt was set to: \(anInt)")
        }
    }
}

var myObject = MyClass()
let myRange = (0...2)
let subscription = myRange.publisher.map{ $0 * 10}
    .assign(to: \.anInt, on: myObject)
```
**Avoiding memory cylces**
When not using the unowned self and weak self this can cause a memory cycle error where the class is never deinitilized and stays in memory, so you always want ot use a `weak self` or `unowned self` 

```swift
    
    init() {
        ///Creating a data stream so we know when our user is updated
        user.map(\.id)
//            .assign(to: \.userID, on: self) /// Causes a memory cycle
            .sink{ [weak self] value in
                self?.userID = value
            }
            .store(in: &subscriptions)
    }
```
