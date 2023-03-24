
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
