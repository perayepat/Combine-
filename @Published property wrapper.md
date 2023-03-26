# Overview 
**@Published**

Its essentially a publisher but you can't send any values to the publisher
@Published vs CurrentValueSubject 

CurrentValueSubject is a value, a publisher and a subscriber all in one.
Sadly it doesn’t fire objectWillChange.send() when used inside an ObservableObject.

```swift

class ViewModel {
    
    // use @Published to create publisher
    @Published var userNames: [String] = ["Bill"]
//    let userNames = CurrentValueSubject<[String], Never>(["Bill"])
    let newUserNameEntered = PassthroughSubject<String, Never>()
    
    var subscriptions = Set<AnyCancellable>()
    
    init() {
        /// Using the dollar sign gives us the pib
        $userNames.sink{ [unowned self] value in
            print("receive value \(value) with \(self.userNames)")
        }
        .store(in: &subscriptions)
        
        newUserNameEntered.sink{ [unowned self] value in
            self.userNames.append(value)
        }
        .store(in: &subscriptions)
    }
}

let viewModel = ViewModel()

//get value
//viewModel.userNames

// add new user name "Susan"
viewModel.newUserNameEntered.send("Susan")

// add new user name "Bob"

//  Documentation: When the property changes, publishing occurs in the property's `willSet` block, meaning subscribers receive the new value before it's actually set on the property.

```
