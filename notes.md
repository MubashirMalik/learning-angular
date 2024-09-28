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