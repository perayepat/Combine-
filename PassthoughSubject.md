# Overview 

As a concrete implementation of Subject, the PassthroughSubject provides a convenient way to adapt existing imperative code to the Combine model.
Unlike CurrentValueSubject, a PassthroughSubject doesn’t have an initial value or a buffer of the most recently-published element. A PassthroughSubject drops values if there are no subscribers, or its current demand is zero.

A publisher that you can continously send new values down 

PassthroughSubject
Use for statring action/ process, equivalent to func 

```swift
/// Does not hold a value
/// It's a different use case for a data flow were you want to start a action

let newUserNameEntered = PassthroughSubject<String, Never>()


// get the value for newUserNameEntered
// Does not hold a value

// subscribe to Subject
let subscription = newUserNameEntered.sink{
    print($0)
} receiveValue: { value in
    print("reveived value \(value)")
}

// passing down new values with Subject
newUserNameEntered.send("BOB")

// sending completion finished with Subject
newUserNameEntered.send(completion: .finished)

```
