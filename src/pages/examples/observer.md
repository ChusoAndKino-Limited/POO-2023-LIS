# Observer pattern


The **Observer Pattern** is a software design pattern that establishes a one-to-one dependency between objects so that when one object changes state, all its dependents are notified and updated automatically¹. This pattern is also known as Dependents or Publish-Subscribe¹. It's often used for implementing distributed event-handling systems in event-driven software². The object that maintains the list of its dependents is called the subject, and the dependents are called observers².

The **RxJS library**, used in the Angular framework, is a powerful tool for implementing this Observer Pattern⁶. RxJS stands for Reactive Extensions for JavaScript, and it allows developers to work with asynchronous data streams and handle complex event-driven systems in a simple and efficient way⁶. With RxJS, developers can easily implement complex functionality such as real-time updates, debouncing, and throttling⁶.

In Angular, RxJS provides an implementation of the Observable type, which is needed until the type becomes part of the language and until browsers support it⁸. The library also provides utility functions for creating and working with observables. These utility functions can be used for converting existing code for async operations into observables, iterating through the values in a stream, mapping values to different types, filtering streams, and composing multiple streams⁸.

## How the example works

Rxjs is also open source, so it will *easy* to find out how the observer is implemented in real life.

![alt](https://raw.githubusercontent.com/ReactiveX/rxjs/master/apps/rxjs.dev/src/assets/images/logos/Rx_Logo_S.png)

https://github.com/ReactiveX/rxjs



---



Source: Conversation with Bing, 9/30/2023
(1) Observer Pattern - Javatpoint. https://www.javatpoint.com/observer-pattern.
(2) Observer pattern - Wikipedia. https://en.wikipedia.org/wiki/Observer_pattern.
(3) 10 Best Angular Libraries for 2023 | Technology Hits - Medium. https://medium.com/technology-hits/10-best-angular-libraries-for-2023-fcac69f6e962.
(4) Angular - The RxJS library. https://angular.io/guide/rx-library.
(5) Understanding Design Patterns: Observer - DEV Community. https://dev.to/carlillo/understanding-design-patterns-observer-2ajp.
(6) The Observer Pattern in Java | Baeldung. https://www.baeldung.com/java-observer-pattern.
(7) Observer Design Pattern in Python - Stack Abuse. https://stackabuse.com/observer-design-pattern-in-python/.
(8) . https://bing.com/search?q=RxJS+library+in+Angular+framework.
(9) Angular - The RxJS library - w3resource. https://www.w3resource.com/angular/the-rxjs-library.php.
(10) Everything You Need To Know About RxJS in Angular With Examples. https://thinkpalm.com/blogs/everything-you-need-to-know-about-rxjs-in-angular-with-examples/.
(11) undefined. https://www.educative.io/blog/rx-js-angular.
(12) undefined. https://indepth.dev/posts/1162/rxjs-in-angular-part-1.
(13) undefined. https://blog.angular-university.io/functional-reactive-programming-for-angular-2-developers-rxjs-and-observables/.
(14) undefined. https://rxjs.dev/.