
# Overview 

The following code shows how to create a basic timer using the combine framework 

``` swift
/// This is a publihser alone
/// Assigning it to a value allows you to hold on to your subscription
var subscription : Cancellable? = Timer.publish(every: 1, on: .main, in: .common)
    .autoconnect()
//    .print("Stream")
///Dealing with too many values in the stream and minimize the data you want to process
    .throttle(for: .seconds(1), scheduler: DispatchQueue.main, latest: true)
///Scan allow you to create a timer here but can be used for other things using the intitial value and the value you recieve in the strem
    .scan(0, { count, _ in
        return count + 1
    })
    .filter{ $0 > 5 && $0 < 15 }
    .sink { (output) in
        print("Finished stream with \(output)")
    } receiveValue: { value in
        print("Received value \(value)")
    }


RunLoop.main.schedule(after: .init(Date(timeIntervalSinceNow: 25))){
    print("--- cancel")
//    subscription.cancel()
    subscription = nil
    
}
```
