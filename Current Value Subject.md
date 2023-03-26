# Overview 

Unlike `PassthroughSubject`, `CurrentValueSubject` maintains a buffer of the most recently published element. 

Calling `send(_:)`on a `CurrentValueSubject` also updates the current value, making it equivalent to updating the value directly.

```swift
struct User {
    var id: Int
    var name: String
}

let currentUserId = CurrentValueSubject<Int     , Never>(1000)
let userNames     = CurrentValueSubject<[String], Never>(["Bob", "Susan", "Luise"])
let currentUser   = CurrentValueSubject<User    , Never>(User(id: 1, name: "Bob"))

//get the value of the currentUserID

print("currentUserId \(currentUserId.value)")

// Subscribe to subject
let subscription = currentUserId
    .sink{
        print("Completion \($0)")
    } receiveValue: {value in
        print("receieve value \(value)")
    }

// passing down new value with subject
/// The moment the user taps on item on a list we would send the current user downstream
/// The subjects are useful for starting streams from user interactions
currentUserId.send(1)
currentUserId.send(2)

//sending completion finished with subject
currentUserId.send(completion: .finished) /// This allows you to stop all the sreams coming into the object 

currentUserId.send(100)

```
