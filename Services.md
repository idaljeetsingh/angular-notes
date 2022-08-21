# Services

Services are objects that can be made available to any component of an Angular application.

## Dependency Injection

Dependency Injection is a programming practice for creating objects.

In Angular, by default the classes are not injectable. There are 3 ways to make a class injectable.
For making the class injectable regardless of the way they are injected, the class is annotated with the `@Injectable`
decorator.

1. Using Services:

   The providedIn option for `@Injectable` decorator tells Angular where to expose the service.
   By default, it is set to `root` and is made available for injection in any component of the app, but it can be
   changed to limit the scope of injection.

   This type of injection is usually called global level.

   For Example:

   ```typescript
   @Injectable({
       providedIn: "root"
   })
   export class SomeService {
   }
   ```

2. Using Modules:

   For this, first remove the providedIn option from the @Injectable decorator and then add the Service to inject in
   the `providers` array of `@NgModule` decorator.

   This type of injection is called Module level injection

3. Using Components:

   For this, first remove the providedIn option from the @Injectable decorator and then add the Service to inject in the
   `providers` array of the `@Component` decorator.

   This type of injection is called component level injection.

## Singleton

Singleton is a design pattern in programming world when a single instance of a class is given to an application.

By default, Angular will treat services as singletons.

## Content Children Decorator

`@ContentChildren` decorator allows us to select elements from projected content.

For example:

```typescript
export class Services {
    @ContentChildren(SomeComponent) children = {};
}
```

