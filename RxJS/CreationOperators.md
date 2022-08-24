# Creation Operators

These operators help in creating an observable by using the declarative approach.

## Timing Operators

### Interval Operator

To simply get a countdown or perform a task with a defined interval, the interval operator is used.

For Example:

```javascript

import {interval} from "rxjs";

const observable = interval(1000); // 1000 is the interval in ms


const subscription = observable.subscribe({
    next: (value) => {
        console.log(value);
    },
})

setTimeout(() => {
    subscription.unsubscribe()
}, 4000);
```

To simplify the code even further, it can be re-written as:

```javascript

import {interval} from "rxjs";

const observable = interval(1000); // 1000 is the interval in ms


const subscription = observable.subscribe(
    console.log
)

setTimeout(() => {
    subscription.unsubscribe()
}, 4000);
```

### Timer Operator

Timer operator provides more flexibility than the `interval` operator. It emits a stream of numbers after the
duration/time value specified as the first parameter. The second parameter of the timer() is the iteration interval.

For example, to start a stream of numbers and get a value after every second, below code can be referred:

```javascript

import {timer} from "rxjs";

const observable = timer(0, 1000);


const subscription = observable.subscribe(
    console.log
)

setTimeout(() => {
    subscription.unsubscribe()
}, 4000);
```

## DOM Events Operator

DOM events operator tracks and gives the DOM events wrapped in an observable.

### FromEvent Operator:

This operator allows us to use DOM events. It has 2 arguments:

1. Target - It refers to the element in the document to watch
2. Event - Name of the event to watch

For Example:

```javascript

import {fromEvent} from "rxjs";

const observable = fromEvent(document, 'click');


const subscription = observable.subscribe(
    console.log
)

setTimeout(() => {
    subscription.unsubscribe()
}, 4000);
```

**Note:** The `fromEvent` operator does not automatically stop listening for events.

## of & from Operators

These operators allow us to loop through values synchronously.

### of Operator

`of` operator can have unlimited number of arguments. It always completes the observable after iterating through all the
values provided to it.

```javascript

import {of} from "rxjs";

const observable = of(1, 2, 3, 4, 5);

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

**Note:** The `of` operator can only loop through simple types like numbers, strings etc. On passing a complex type
such as an array, it will treat the whole array as 1 single element and not traverse/iterate the elements inside the
array.
Example:

```javascript
import {of} from "rxjs";

const observable = of([1, 2, 3, 4, 5]);


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

The above example will give the output as:

```
[1, 2, 3, 4, 5]
of Operator completed
This will be logged after of operator completes.
```

To tackle such issue, we use the `from` operator.

### from Operator

`from` operator can receive complex types and flatten them to iterate over the nested data.

For Example:

```javascript
import {from} from "rxjs";

const observable = from([1, 2, 3, 4, 5]);

const subscription = observable.subscribe({
    next(value) {
        console.log(value);
    },
    complete() {
        console.log('from Operator completed')
    }
})

console.log('This will be logged after from operator completes.')

setTimeout(() => {
    subscription.unsubscribe()
}, 4000);
```

The above example will give the output as:

```
1
2
3
4
5
from Operator completed
This will be logged after from operator completes.
```

**Note:** from operator can even flatten the simple type like a string. Passing a string to from operator will break it
down to array of characters.

For example:

```javascript
import {from} from "rxjs";

const observable = from('Hello ');

const subscription = observable.subscribe({
    next(value) {
        console.log(value);
    },
    complete() {
        console.log('from Operator completed')
    }
})

console.log('This will be logged after from operator completes.')

setTimeout(() => {
    subscription.unsubscribe()
}, 4000);
```

The above example will give the output as:

```
H
e
l
l
o
from Operator completed
This will be logged after from operator completes.
```

