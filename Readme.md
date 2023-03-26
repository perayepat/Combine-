#Overview 

How to create a subscription?
- sink{ completion: , receiveValue: }
- assign(to: , on: )
- assign()

How to stop a data stream?
- subscription.cancel
- subscription = nil 
- error will cancel subscription with failure 

sequence publihser, one-shot publisher
- finish automatically when done 

--- Multithreading in Combine ---
- reveive on 
- subscribe to 
- where is multithreading built in? - urlsession
