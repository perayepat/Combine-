# Overview 

**How to create a subscription?**
- sink{ completion: , receiveValue: }
- assign(to: , on: )
- assign()

**How to stop a data stream?**
- subscription.cancel
- subscription = nil 
- error will cancel subscription with failure 

**sequence publihser, one-shot publisher**
- finish automatically when done 

**Multithreading in Combine**
- reveive on 
- subscribe to 
- where is multithreading built in? - urlsession

# Publishers and Starting Streams
How to pass values into a Combine data stream?
 Publisher: the someone who emit values

===== Key parts of a publisher =========
 
protocol Publisher {
   associatedtype Output
   associatedtype Failure: Error
  
   func receive<S>(subscriber: S) where S : Subscriber,
                                        Self.Failure == S.Failure,
                                        Self.Output == S.Input
}

====== continously emitting values =====
 
---- Subject ----
CurrentValueSubject<Int, Never>(1000)
PassthroughSubject<Int, Never>()
 
---- SwiftUI specific ----
@Published                -> property wrapper with added CurrentValueSubject
ObservableObject Protocol -> objectWillChange PassthroughSubject
 
 
 
===== limited-values publishers =======
 
 Sequence  [1,2,3,4].publisher
 Just
 Future - Ad-hoc callbacks (completion closures for callbacks)
 Deferred
 Empty
 Fail
 Record
 Optional(nil).publisher
 
 
 ---- Foundation Framework ----

 Timer publisher
 URLSession dataTaskPublisher
 NotificationCenter publisher
 publisher on KVO instance - replacing target/action patterns
 
 
 === type system of Publishers ===
 
 type erasing:
 AnyPublisher
 .eraseToAnyPublisher()
 
 use for func that return publishers
 
