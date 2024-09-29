# Notes

### What is Angular?
- frontend JS framework
- collection of tools & features (CLI, Debugging Tools, IDE Plugins)
### Why use Angular?
- Simplify process of building complex, highly interactive user interfaces
- Write **declarative** code rather than imperative code.
- Define the target UI states & how they change and let Angular do the rest!
- Separation of concerns using **components** (like React)
    - modular applications
    - better development process
- embraces some OOP concepts & principles
- Uses Typescript

**NOTE:** Angular 1 or Angular JS is totally different framework. 

### Commands
```python
ng new project-name # like nest new project-name
npm start # ng serve
ng serve
ng g c header # ng generate component header
```
### index.html
- Loaded by the browsers when visitor visit the website.

- But, it is almost empty with only one strange html element `<app-root></app-root>` in body?

- When website loads up, the code in `main.ts` file is executed.

- But, there are no imports (e.g., `main.ts`) or script tags in `index.html`.

- But, in page source we can see script tags which are automatically inserted by Angular CLI.

### Component

```ts
// File Naming Conventions
// header.component.html
// header.component.css
// header.component.ts
// And put in header folder.

@Component({
    selector: 'app-header', // convention is to use at least two words. header can clash with HTML built-in tag.
    template: '<h1>Hello World</h1>' // Not recommended,
    templateUrl: './header.component.html',
    standalone: true, // recommended to use true, module = false
    styleUrl: './header.component.css' // stylesUrl, styles
})
export class HeaderComponent

// How to use/import?
@Component({
    imports: [HeaderComponent]
})
export class AppComponent

```

### How to use components?
- Components can be used in this way but it is not how we do it. 
- Idea is to build a tree of components.
- **Why?** Exchange data and communicate with each other.
- **Then How?** Call bootstrap only once and use other components inside app/root component.
```ts
import { bootstrapApplication } from '@angular/platform-browser'
// ... imports

bootstrapApplication(AppComponent);
bootstrapApplication(HeaderComponent);
```

### Important Concept Regarding CSS
- A component can only see its own styles. For example, styles defined in `header.component.css` can only be seen by `HeaderComponent` and not by `AppComponent`.

- Above point maybe easy to understand as header component is a child of app componet so its style are not visible to app component.

- But, what about styles defined in `app.component.css`? Can they be seen by `HeaderComponent`? **No!** Because, Angular uses <mark>Shadow DOM</markj> to encapsulate styles.

### Dynamic Content & Getters For Computed Values
All properties defined in component class are available in template of that class.

```js
// String interpolation: access any public property of class
{{ selectedUser.name }}

<img src="{{ selectedUser.avatar }}" />

// Property Binding: Skip double curly brackets when configuring an element through its attributes
<img [src]="selectedUser.avatar" [alt]="selectedUser.name"/>
<img [src]="'assets/user/' + selectedUser.avatar" />

// Getters for computed values
get imagePath () {
    return 'assets/user/' + this.selectedUser.avatar
}
```
### State
- So easy.. no setState hanky panky!!
- Uses zone.js behind the scene to detect changes

### Signals
- looks like retarded useState
- trackable data containers
- an object that stores a value (any type of value)
- notify Angular when values change so Angular can update parts of UI where those values are used

### Topics to Revisit/Revise
- Video: 28 - on signals