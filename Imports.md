# Imports

Angular requires various imports so that the app can run properly and many other reasons.

Below are some points/practices that needs to be followed while developing an Angular application:

1. The `CommonModule` exports components, directives and pipes. For Example, ngIf directive is exported from this
   module.
2. The `BrowserModule` works similar to the `CommonModule` but the main difference between them is that `BrowserModule`
   also provides some additional services to run the app in the browser.
3. The `BrowserModule` should always be imported in the root/app module and all the other modules must import
   the `CommonModule`.
4. To make a component accessible inside some other component/module, it must be exported. The CLI by default
   does not add the export for the component created using `ng g c <component-name>` command.
5. 
