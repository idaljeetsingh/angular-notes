# Content Transformation

## Pipes

A pipe is a function for transforming the value of an object.
Pipes can be defined once and used everywhere. Not all pipes are configurable and hence documentation must be referred
before using pre-built pipe.

Pipes does not modify the actual value in the class. It only modifies the value after it's being evaluated in the
template.
For using pipes, it has to be imported in the app from the 'BrowserModule' which is imported by default in the root
AppModule.

To use a pipe, simply add a pipe (|) character after the value that has to be modified for display.

### TitleCase Pipe

This pipe is not configurable and does not receive any parameters.

Example:

```angular2html
<p> This {{ randomValue | titlecase }} is modified by title case pipe</p>
```

### Date Pipe

The Date pipe transforms the date object by receiving a parameter

Example:

In the class:

```typescript
currentDate = new Date();
```

And in the HTML:

```angular2html
<p>Current Date: {{currentDate | date: 'MMMM d'}}</p>
<p>Current Date: {{currentDate | date: 'short'}}</p>
```

This will modify the date on webpage with the pattern specified.

### Currency Pipe:

Modifies the numeric value and adds the currency symbol & decimal values.
It simply modifies the value and does not actually convert the value to the currency specified.

Example:

```angular2html
<!--Currency codes can be found in ISO 4217 on internet-->
<p>The total cost is: {{cost | currency}}</p>
<p>The total cost is: {{cost | currency: 'JPY'}}</p>
<p>The total cost is: {{cost | currency: 'INR'}}</p>
```

The above examples will ony add the currency symbol to the value and will not actually convert it from let's say USD to
INR.

[More on Angular Pipes can be found here](https://angular.io/api?type=pipe)

## Directives

Also called custom attributes for transforming content.
There are 2 types of directives:

- Attribute Directives: Changes the appearance or behaviour of an element
- Structural Directives: Add or remove elements from the DOM

Both the directives must be wrapped with [] for property binding otherwise we won't be able to use an expression as a
value .

### ngClass Directive

It is an 'Attribute Directive' and can change appearance of elements by adding a whole CSS class based on some
conditions.
To add only a simple CSS attribute and not the whole class, use the ngStyle directive.

Example:

In the component, maintain the state for the class.

```typescript
blueClass: false;
```

In the CSS:

```css
.blue {
    background-color: blue;
    color: white;
}
```

In the HTML:

```angular2html

<button
        (click)="blueClass = !blueClass"
        [ngClass]="{blue: blueClass}"
></button>
```

[More on Official Angular directives can be found here](https://angular.io/api?type=directive)

### ngTemplate Directive

It is a 'Structural Directive' to store one or more elements in the memory. By wrapping the content with this directive,
Angular will not render the contents immediately but instead hold them in the memory.
In the HTML code present in browser, a comment with label 'Container' is added and the content held by this directive is
rendered in place of that comment.

<strong> To display the contents of this element, another structural directive 'ngIf' is used. </strong>

Example:

```angular2html

<ng-template [ngIf]="blueClass">
    <p> The button is now blue.</p>
</ng-template>
```

This way can be very tedious as there will be multiple ngTemplate elements in our HTML and the code will then look
dirty.

Instead, we can apply the structural directives directly on the HTML elements like a <p> tag and prefix the structural
directive with an asterisk (*).

Example:

The above code can be refactored as:

```angular2html
<p *ngIf="blueClass">The button is now blue.</p>
```

By adding an asterisk (*), Angular performs 2 actions:

1. Wrap the element with <ng-template> element.
2. The value will be interpreted as an expression.

<strong> Using * makes it easier to identify the structural directives. </strong>

[More shorthand examples can be found here](https://angular.io/guide/structural-directives#shorthand-examples)

### ngFor Directive

It is also a Structural Directive used for looping through an element with an array.

ngFor follows a special syntax as:
```*ngFor="let <element> of <ArrayElements>"``` where the values wrapped with <> will be dynamic.

To get the index as well along with the iteration over the array elements, the syntax will be:
```*ngFor="let <element> of <ArrayElements>; let <indexVariable> = index"```. Here the indexVariable will be assigned
the index property from the ngFor directive.



