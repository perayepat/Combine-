
# Overview

When the publisher has run out of items to publish it will automatically stop 
``` swift
let foodbank: Publishers.Sequence<[String], Never> = ["apple","bread","orange","milk"].publisher


///
let foodSubscription = foodbank.sink { (completion) in
    print("completion: \(completion)")
} receiveValue: { foodItem in
    print("receive item \(foodItem)")
}
```

## Zip
this is combine two sequences in this case publishers into a tuple 
the zip will accomodate for the shortest sequence bewtween two pairs

``` swift
let foodbank: Publishers.Sequence<[String], Never> = ["apple","bread","orange","milk"].publisher
var timer = Timer.publish(every: 0.5, on: .main, in: .common).autoconnect()



let subscription = foodbank.zip(timer).sink { (completion) in
    print("completion: \(completion)")
} receiveValue: { (foodItem,timestamp) in
    print("recieve item \(foodItem) at \(timestamp.formatted(date: .omitted, time: .standard))")
}

```
**output**
```
recieve item apple at 5:53:07 PM
recieve item bread at 5:53:08 PM
recieve item orange at 5:53:08 PM
recieve item milk at 5:53:09 PM
completion: finished
```
