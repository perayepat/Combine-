# Overview 

Unlike `PassthroughSubject`, `CurrentValueSubject` maintains a buffer of the most recently published element.
Calling `send(_:)`on a `CurrentValueSubject` also updates the current value, making it equivalent to updating the value directly.
