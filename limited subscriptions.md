
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
