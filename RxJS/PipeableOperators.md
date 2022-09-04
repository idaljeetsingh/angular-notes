# Pipeable Operators

These are the functions for handling streams of data and can transform, filer & combine the data.
They take an observable as an input and outputs a new observable.

Pipeable operators make up a huge portion of the RxJS Library.

The output of a pipeable operator is another observable and will push the data applied by the new observable.

Example usage:

```typescript
const observable = new Observable()
observable.pipe(
    firstOperator(config),
    secondOperator(config)
)
```

## Map Operator

Map operator receives the value provided by the observable, modifies it and then returns the modified value to be used
wherever required.

Example:

```typescript
import {of} from "rxjs";
import {map} from "rxjs/operators"

const observable = of(1, 2, 3, 4, 5).pipe(
    map((value) => `$${value}`)
);


const subscription = observable.subscribe({
    next(value) {
        console.log(value);
    },
    complete() {
        console.log('Of Operator completed')
    }
})

console.log('This will be logged after of operator completes.')

setTimeout(() => {
    subscription.unsubscribe()
}, 4000);
```

## Pluck Operator

[***Deprecated***]

Allows us to grab a specific value/property from an object. Although, same can be achieved by map operator but pluck
operator makes it easy and neat.

Example Using map operator:

```typescript
import {fromEvent} from "rxjs";
import {map} from "rxjs/operators"

const observable = fromEvent(document, 'keydown').pipe(
    map(event => event.code)
);


const subscription = observable.subscribe({
    next(value) {
        console.log(value);
    }
})

setTimeout(() => {
    subscription.unsubscribe()
}, 4000);
```

Same code using pluck operator:

```typescript
import {fromEvent} from "rxjs";
import {pluck} from "rxjs/operators"

const observable = fromEvent(document, 'keydown').pipe(
    pluck('code')
);

const subscription = observable.subscribe({
    next(value) {
        console.log(value);
    }
})

setTimeout(() => {
    subscription.unsubscribe()
}, 4000);
```

## Filter Operator

It can be used to stop an observable from pushing value based on some condition.

Example:

```typescript
import {fromEvent} from "rxjs";
import {pluck, filter} from "rxjs/operators"

const observable = fromEvent(document, 'keydown').pipe(
    pluck('code'),
    filter(code => code === 'Space')
);

const subscription = observable.subscribe({
    next(value) {
        console.log(value);
    }
})

setTimeout(() => {
    subscription.unsubscribe()
}, 4000);
```

Console Output on pressing various keys including SpaceBar key:

```
Space
Space
Space
Space
```

## Reduce Operator

Similar to Array.reduce() but applicable for Observables.

Example

```typescript
import {of} from "rxjs";
import {reduce} from "rxjs/operators";

const observable = of(1, 2, 3, 4, 5).pipe(
    reduce((accumulator, value) => accumulator + value, 0) // Accumulator stores value previously returned by this function
)

const subscription = observable.subscribe({
    next(value) {
        console.log(value);
    },
    complete() {
        console.log('Of Operator completed')
    }
})

console.log('This will be logged after of operator completes.')

setTimeout(() => {
    subscription.unsubscribe()
}, 4000);
```

## Take Operator

It is used to limit the values pushed by the observables. It accepts number of values that can be pushed from an
Observable.

Example

```typescript
import {interval} from "rxjs";
import {take, reduce} from "rxjs/operators";

const observable = interval(500).pipe(
    take(5),
    reduce((accumulator, value) => accumulator + value, 0) // Accumulator stores value previously returned by this function
)

const subscription = observable.subscribe({
    next(value) {
        console.log(value);
    }
})

setTimeout(() => {
    subscription.unsubscribe()
}, 4000);
```

## Scan Operator

Scan Operator is an alternative to reduce operator but with the feature to pass on the value to the operator in chain.

Example:

```typescript
import {interval} from "rxjs";
import {take, scan} from "rxjs/operators";

const observable = interval(500).pipe(
    take(5),
    scan((accumulator, value) => accumulator + value, 0) // Accumulator stores value previously returned by this function
)

const subscription = observable.subscribe({
    next(value) {
        console.log(value);
    }
})

setTimeout(() => {
    subscription.unsubscribe()
}, 4000);
```

The above code will generate following output on the console

```
0
1
3
6
10
```

## Tap Operator

Tap operator is used for debugging the pipeline without affecting the stream or value.
The Tap Operator will be given te value emitted by the previous observable or operator.

Example:

```typescript
import {interval} from "rxjs";
import {take, tap, reduce} from "rxjs/operators";

const observable = interval(500).pipe(
    take(5),
    tap(console.log),
    reduce((accumulator, value) => accumulator + value, 0) // Accumulator stores value previously returned by this function
)

const subscription = observable.subscribe({
    next(value) {
        console.log(value);
    }
})

setTimeout(() => {
    subscription.unsubscribe()
}, 4000);
```

The above code will generate the output as

```
0
1
2
3
4
10
```

Apart from normal logging, we can also pass an object to Tap operator with `next`, `error` and `complete` functions.

For example:

```typescript
import {interval} from "rxjs";
import {take, tap, reduce} from "rxjs/operators";

const observable = interval(500).pipe(
    take(5),
    tap({
        next(val) {
            console.log(val)
        }
    }),
    reduce((accumulator, value) => accumulator + value, 0) // Accumulator stores value previously returned by this function
)

const subscription = observable.subscribe({
    next(value) {
        console.log(value);
    }
})

setTimeout(() => {
    subscription.unsubscribe()
}, 4000);
```

The above code will generate the output as normal tap function, but it shows that we can also pass observer lke objects
to Tap operator.

```
0
1
2
3
4
10
```

## Flattening Operators

A flattening operator can subscribe to an observable returned from within an observable.
The subscriptions are handled within the pipeline and not inside the observer.

### MergeMap Operator

This operator is similar to the Map operator but can subscribe to the observable returned from the process/block.

Example

```typescript
import {fromEvent} from "rxjs";
import {mergeMap} from "rxjs/operators";
import {ajax} from "rxjs/ajax";

const button = document.querySelector('#btn');

const observable = fromEvent(
    button, 'click'
).pipe(
    mergeMap(() => {
        return ajax.getJSON('https://jsonplaceholder.typicode.com/todos/1')
    })
)

const subscription = observable.subscribe({
    next(value) {
        console.log(value);
    }
})

setTimeout(() => {
    subscription.unsubscribe()
}, 4000);
```

This will log the value returned by the observable of `ajax` operator as `mergeMap` operator subscribes to it and emits
for further use.

***Note 1:*** The inner observables returned/created from nested blocks must be explicitly handled/stopped otherwise it
may lead to Memory Leaks.

***Note 2:*** The mergeMap Operator does not limit/restrict the number of active inner observables.

### SwitchMap Operator

The switchMap operator is similar to mergeMap operator and can subscribe to an inner observable.

***Note:*** The switchMap Operator imposes limit/restriction on the number of active inner observables to ***1***.
When an active inner observable is running and a new observable comes in light, the current active observable is
completed and the new observable becomes active.

Example

```typescript
import {fromEvent, interval} from "rxjs";
import {switchMap, take, tap} from "rxjs/operators";

const button = document.querySelector('#btn');

const observable = fromEvent(
    button, 'click'
).pipe(
    switchMap(() => {
        return interval(1000).pipe(
            take(5),
            tap({
                complete() {
                    console.log("Inner observable completed")
                }
            })
        )
    })
)

const subscription = observable.subscribe({
    next(value) {
        console.log(value);
    }
})

setTimeout(() => {
    subscription.unsubscribe()
}, 40000);
```

A practical scenario for using the `switchMap` operator is for handling frequent http requests and maintain consistency
across the application.

### ConcatMap Operator

ConcatMap operator also limits the active inner observables to 1 but the main feature of this operator is that on
creation of a new observable, instead of cancelling/completing the previous observable, it waits for it to complete and
pushes the newly created observable inside a queue. The next observable inside the queue is not subscribed to until the
current observable is completed.

Example

```typescript
import {fromEvent, interval} from "rxjs";
import {concatMap, take, tap} from "rxjs/operators";

const button = document.querySelector('#btn');

const observable = fromEvent(
    button, 'click'
).pipe(
    concatMap(() => {
        return interval(1000).pipe(
            take(5),
            tap({
                complete() {
                    console.log("Inner observable completed")
                }
            })
        )
    })
)

const subscription = observable.subscribe({
    next(value) {
        console.log(value);
    }
})

setTimeout(() => {
    subscription.unsubscribe()
}, 40000);
```

### ExhaustMap Operator

ExhaustMap operator also subscribes to inner observables and limits the active inner observable count to 1.
The main usage of `exhaustMap` is to simply ignore the subsequent new observables that may get created in a pipeline.


