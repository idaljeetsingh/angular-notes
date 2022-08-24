# Fundamentals of RxJS

RxJS is a library for filtering, sorting & coordinating data.

## Key Terms Observables

- **Observables**: They are wrappers around a datasource.

- **Observers**: They are responsible to receive the data after an observable has emitted the data. Multiple observers
  can subscribe to an observable.

- **Subscription**: Observer establishing connection with an observable is called a subscription.

## Hello World using RxJS

A simple Hello World can be logged in browser using RxJS with the following code:

```javascript
import {Observable} from "rxjs";

const observable = new Observable((subscriber) => {
    subscriber.next("Hello World from RxJS!");
});

observable.subscribe({
    next: (value) => {
        console.log(value);
    }
})
```

## Observers

An observer is created if we subscribe to an observable.
The job of the observer is to read the value as they are pushed (sent from next method).

The object we pass to subscribe() can have upto 3 functions in it as:

1. **next** -> to handle emitted values
2. **error** -> to handle the errors
3. **completion** -> to handle the completion of the observable

Observables run continuously and are not terminated automatically. The termination is to be performed manually
by us otherwise Memory Leakage starts.
When the observable is completed, the return() method is invoked and can be used to perform additional cleanups
after completion of the observable like clearing intervals or handling memory leaks from the observables.

## Terminating the observable

To manually terminate the observable, complete method is called on the observable object.

For Example:

```javascript
import {Observable} from "rxjs";

const observable = new Observable((subscriber) => {
    subscriber.next("Hello World from RxJS!");
    subscriber.next("This is another message from RxJS..");

    subscriber.complete();
    subscriber.next("This message won't be pushed as complete() was called above this statement.");

});

observable.subscribe({
    next: (value) => {
        console.log(value);
    },
    complete: () => {
        console.log("This will be logged once the observable has completed.")
    }
})
```

The above code explains manual completion of an observable.

**Note:** Completing an observable does not necessarily mean that the contents inside the observable are also
cleared/completed.</strong> In Many cases like setInterval(), the method or content will keep on executing even when the
complete method is called on the observable.

## Errors in observables

Errors can also occur in an observable and when there is an error, it stops emitting new values and the complete
function is also not called as an error has occurred.

To handle the errors in an observer:

```javascript
import {Observable} from "rxjs";

const observable = new Observable((subscriber) => {
    subscriber.next("Hello World from RxJS!");
    subscriber.error("Error occurred in observable.. new messages won't be pushed")
    subscriber.next("This is another message from RxJS..");
    subscriber.complete();
    subscriber.next("This message won't be pushed as complete() was called above this statement.");

});

observable.subscribe({
    next: (value) => {
        console.log(value);
    },
    complete: () => {
        console.log("This will be logged once the observable has completed.")
    },
    error: (err) => {
        console.error(err)
    }
})
```

The above code example will only log the first two statements i.e. one Hello World & the other one with error log.

## Note:

The example used here is a fully synchronous operation and can be verified with the example:

```javascript
import {Observable} from "rxjs";

const observable = new Observable((subscriber) => {
    subscriber.next("Hello World from RxJS!");
    subscriber.complete();
});

console.log("Test log statement.. ")

observable.subscribe({
    next: (value) => {
        console.log(value);
    },
})

console.log("Ending log statement")
```

The response of the above code will be in this sequence:

```
Test log statement.. 
Hello World from RxJS!
Ending log statement
```
