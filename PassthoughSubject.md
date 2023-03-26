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

# Multiple Publisher Example

You don't need to use a didset anymore because now you can just subscribe to the event 

```swift
class ViewModel {
    
   private let userNamesSubject = CurrentValueSubject<[String], Never>(["Bill"])
    var userNames: AnyPublisher<[String], Never>
    
    let newUserNameEntered = PassthroughSubject<String, Never>() /// This is going to be used to add and change user names
    
    
    var subscriptions = Set<AnyCancellable>()
    
    init() {
        // create publisher stream that updates userNames whenever a newUserNameEntered has a new value
        userNames  = userNamesSubject.eraseToAnyPublisher()
        
        newUserNameEntered
            .filter{$0.count >= 3}  /// protect how new data making sure it has the right requirements
            .sink{ _ in
            
        } receiveValue: { [unowned self] username in
//            userNames.value = userNames.value + [username] /// Option One
            self.userNamesSubject.send(userNamesSubject.value + [username])
        }
        .store(in: &subscriptions)
        
        userNames.sink{ users in
            print("usernames changed to \(users)")
        }
        .store(in: &subscriptions)
        
    }
}

let viewModel = ViewModel()
 
// add new user name "Susan"
viewModel.newUserNameEntered.send("Susan")

// add new user name "Bob"
viewModel.newUserNameEntered.send("Bob")

// how do you protect userName from not setting it directly
//viewModel.userNames.send("")


```
