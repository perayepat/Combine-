# Overview 

When using the Observableobject protocol, an extra object of `objectWillChange` is added with a hidden type 

SwiftUI uses `the objectWillChange` Publisher to update the View when you store an object as @ObservedObject
(or @StateObject or @EnvironmentObject) on the view.

You can also manually send updates through objectWillChange using its send() method when you need to manually update the view.

```swift
//: [Previous](@previous)

//SwiftUI works with ObservableObject Protocol

import Foundation
import Combine
import PlaygroundSupport

PlaygroundPage.current.needsIndefiniteExecution = true

class ViewModel: ObservableObject {
  
    @Published var userNames = ["Bill", "Susan", "Bob"]
    let userNamesSubject = CurrentValueSubject<[String], Never>(["Bill", "Susan", "Bob"])
    var subscriptions = Set<AnyCancellable>()
    
    init() {
        //subscribe to publisher
//         $userNames.sink { [unowned self] (values) in
//            print("usernames - last \(self.userNames) -  value received - \(values)")
//         }.store(in: &subscriptions)

        userNamesSubject.sink { [unowned self] values in
            print("usernamesSubject - last \(self.userNamesSubject.value) - value received - \(values)")
            self.objectWillChange.send() // Manually update the view
        }.store(in: &subscriptions)

    }
}

let viewModel = ViewModel()

// where you can see this used internally in SwiftUI
// ViewModel class conforms to ObservableObject protocol
 //let objectWillChange = PassthroughSubject<Void, Never>()

//viewModel.objectWillChange.send()
let subscription = viewModel.objectWillChange.sink { _ in
    print("ObjectWillChange was sent")
}

//viewModel.userNames.append("Karen")
viewModel.userNamesSubject.send(["Karen"])

//: [Next](@next)

```
