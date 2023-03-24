
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

## The Completion
the completion allows us to handle errors in our stream or pipline and heres a code example of how we can handle errors in our stream
these errors will stop our stream

``` swift
///Publisher that will pass limited number of values
let foodbank: Publishers.Sequence<[String], Never> = ["apple","bread","orange","milk"].publisher
var timer = Timer.publish(every: 1, on: .main, in: .common).autoconnect()

let calendar = Calendar.current
let endDate = calendar.date(byAdding: .second, value: 3,to: .now)!

struct MyError: Error{}
func throwErrorEndDate(foodItem: String, date: Date) throws -> String{
    if date < endDate{
        return "\(foodItem) at \(date)"
    } else {
        throw MyError()
    }
}

let subscription = foodbank.zip(timer)
    .tryMap({ (foodItem,timeStamp) in
//        return "recieve item \(foodItem) at \(timeStamp.formatted(date: .omitted, time: .standard))"
        try throwErrorEndDate(foodItem: foodItem, date: timeStamp)
    })
    .sink { (completion) in
    switch completion{
    case .finished:
        print("Comletion: finished")
    case .failure(let error):
        print("completion with failure: \(error.localizedDescription)")
    }
} receiveValue: { (result) in /// changed because we return a single string in the trymap
    print("recieve result \(result)")
}
```
**output**
```
recieve result apple at 2023-03-24 16:04:39 +0000
recieve result bread at 2023-03-24 16:04:40 +0000
completion with failure: The operation couldn’t be completed. (__lldb_expr_44.MyError error 1.)

```
