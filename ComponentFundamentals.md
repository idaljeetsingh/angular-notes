# Component Fundamentals

## Property Biding

Updating the data in html from the ts dynamically.
wrap the html attribute with [] and specify the variable present in the .ts

Example:

In the Component class:

```typescript
import {Component} from "@angular/core";

@Component({
    selector: 'app-root'
})
export class AppComponent {
    imgUrl = 'link-to-a-url';
}

```

In the HTML:

```angular2html
<img [src]="imgUrl">
```

## Events

Events are written as attributes in elements surrounded by ().

Example:

In class create a method:

```typescript
let imgUrl;

function changeEvent(event: KeyboardEvent) {
    // change the imgUrl from the received event  
    this.imgUrl = (event.target as HTMLInputElement).value
}
```

```angular2html
<input (keyup)="changeEvent($event)" [value]="imgUrl" ]>
```

## Passing data from Parent Component to Child Component

Use @Input() decorator from the '@anguar/core' package and add it to the variable/property that will receive the value
from the parent component.

Then, in the html, call the component with the selector tag and pass the value for the variable decorated with @Input()
decorator.

Example:

In the component class:

```typescript
import {Component, Input} from "@angular/core";

@Component({
    selector: 'app-images'
})
export class ImagesComponent {
    @Input() imgUrl = '';
}
```

Now, to pass data to this component from a parent component, the usage will be:

```angular2html

<app-images [imgUrl]="someImageUrl"></app-images>
```

We can also specify the alias for the actual property by specifying that in the @Input() decorator.

Example:

```typescript
import {Component, Input} from "@angular/core";

@Component({
    selector: 'app-images'
})
export class ImagesComponent {
    @Input('img') imgUrl = ''; // Alias is created by specifying the name as a string parameter in @Input() decorator. 
}
```

The usage after creating an alias now becomes:

```angular2html

<app-images [img]="someImageUrl"></app-images>
```

This is because the parent component is now forced to use the alias to bind the property.

<strong>Note: </strong> The angular style guide suggests to avoid aliasing inputs & outputs.

## Passing data from Child Component to Parent Component

Use @Output() decorator from the '@anguar/core' package along with EventEmitter and add it to the variable/property that
will send the value to the parent component.

In the Component class:

```typescript
import {Component, Input, EventEmitter, Output} from "@angular/core";

@Component({
    selector: 'app-images'
})
export class ImagesComponent {
    @Input() imgUrl = '';
    @Output() imgSelected = new EventEmitter<string>();
}
```

In the HTML:

```angular2html
<img [src]="imgUrl" (click)="imgSelected.emit(imgUrl)">
```

In the Parent Component's HTML:

```angular2html

<app-images [imgUrl]="someImageUrl" (imgSelected)="callAMethodInComponent($event)"></app-images>
```

<strong>Note: </strong> Don't prefix the output event/properties names.

## Content Projection

We can use <ng-content> to pass some additional data while calling a child component inside a parent component.
The process of passing data/content inside a component is called content projection.

Example:

In the parent component's HTML:

```angular2html

<app-images [imgUrl]="someImageUrl" (imgSelected)="callAMethodInComponent($event)">
    <p>This is a caption for image passed to this component.</p>
</app-images>
```

and in the Child component's HTML:

```angular2html
<img [src]="imgUrl" (click)="imgSelected.emit(imgUrl)">

<ng-content></ng-content>
```

By simply adding <ng-content>, Angular looks if any data is passed while calling this component. If there is any data,
it is rendered otherwise it remains empty.

## Life Cycle Hooks

They are the functions that run during events; specifically when the components experience any changes.
Some common Life Cycle Hooks are:

- ngOnInit() : it runs after the data/property binding from parent has completed. So property will change after
  completion of constructor()
- ngOnDestroy()
- ngOnChanges()
- ngDoCheck()

More info about LifeCycle Hooks can be [found here](https://angular.io/guide/lifecycle-hooks).

## Scoped CSS

Scoped CSS is the CSS that is present along with the other files of a component.
It is called scoped because it can only affect/style the HTML present inside that component.

:host can be used in CSS to style the data received via [Content Projection](#content-projection)
