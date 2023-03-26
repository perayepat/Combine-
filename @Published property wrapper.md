# Overview 
**@Published**

Its essentially a publisher but you can't send any values to the publisher
@Published vs CurrentValueSubject 

CurrentValueSubject is a value, a publisher and a subscriber all in one.
Sadly it doesn’t fire objectWillChange.send() when used inside an ObservableObject.

