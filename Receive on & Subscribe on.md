**Receive on**
this allows us to switch when we want to go back to the main thread in our operations 

``` swift
let intSubject = PassthroughSubject<Int, Never>()

let subscription = intSubject
    .map {$0} // expensive map
    .receive(on: DispatchQueue.main) // we come back to the main thread
    .sink(receiveValue: { value in
        print("receive value \(value)")
        print(Thread.current)
    })

intSubject.send(1)

DispatchQueue.global().async {
  intSubject.send(2)
}
```

**Subscribe on**

In contrast with receive(on:options:), which affects downstream messages, subscribe(on:options:) changes the execution context of upstream messages.

```swift

PlaygroundPage.current.needsIndefiniteExecution = true

var cancellables = Set<AnyCancellable>()
let intSubject = PassthroughSubject<Int, Never>()

intSubject
    .subscribe(on: DispatchQueue.global())
  .sink(receiveValue: { value in
    print(value)
    print(Thread.current)
  }).store(in: &cancellables)

intSubject.send(1)
intSubject.send(2)
intSubject.send(3)
```
