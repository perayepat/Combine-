# Overview 

When using the Observableobject protocol, an extra object of `objectWillChange` is added with a hidden type 

SwiftUI uses `the objectWillChange` Publisher to update the View when you store an object as @ObservedObject
(or @StateObject or @EnvironmentObject) on the view.

You can also manually send updates through objectWillChange using its send() method when you need to manually update the view.
