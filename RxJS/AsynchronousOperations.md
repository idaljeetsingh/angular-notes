# Asynchronous Operations

The code samples of observables used in [Basics](Basics.md#Note:) are synchronous, but we can also perform
asynchronous operations in an observable like:

```javascript
import {Observable} from "rxjs";

const observable = new Observable((subscriber) => {

    setInterval(() => {
        subscriber.next("Test Message");
    }, 1000)

    subscriber.complete()
});


observable.subscribe({
    next: (value) => {
        console.log(value);
    },
})
```

The above code is a very basic example of asynchronous operation in an observable.

**Caution:** The above code has a memory leak as the setInterval block still keeps on running even after calling the
complete() method.

To Fix the memory leak, clear the interval inside return() in the observable as:

```javascript
import {Observable} from "rxjs";

const observable = new Observable((subscriber) => {

    const intervalId = setInterval(() => {
        subscriber.next("Test Message");
        console.log("I am causing memory leak here...")
    }, 1000)

    subscriber.complete()

    return () => {
        clearInterval(intervalId);
    }
});

observable.subscribe({
    next: (value) => {
        console.log(value);
    },
})
```

## Unsubscribing the Observables from the Observers

There are many scenarios in which we require that only one observer must stop listening to the observable but the others
must continue to listen to the observable they have subscribed to. However, when calling the complete method in the
observable,
it completely stops the observable from pushing new data.

To tackle this, we use the `unsubscribe()` of the Subscription returned by the `subscribe()` method of the observable.
For Example:

```javascript
import {Observable} from "rxjs";

const observable = new Observable((subscriber) => {

    const intervalId = setInterval(() => {
        subscriber.next("Test Message");
        console.log("I am causing memory leak here...")
    }, 1000)

    return () => {
        clearInterval(intervalId);
    }
});

const subscription = observable.subscribe({
    next: (value) => {
        console.log(value);
    },
})

setTimeout(() => {
    subscription.unsubscribe()
}, 4000);
```
