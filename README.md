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
ng g c my-component --skip-tests
ng g c tasks/task --skip-tests
```
### index.html
- Loaded by the browsers when visitors visit the website.

- But, it is almost empty with only one strange html element `<app-root></app-root>` in body?

- When website loads up, the code in `main.ts` file is executed.

- But, there are no imports (e.g., `main.ts`) or script tags in `index.html`.

- But, in page source we can see script tags which are automatically inserted by Angular CLI.

### Component (Separation of Concerns)
- Every component should do one thing.
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
    // For modules set standalone to false
    standalone: true, // standalone components are recommended. 
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
```ts
import { bootstrapApplication } from '@angular/platform-browser'
// ... imports

bootstrapApplication(AppComponent);
bootstrapApplication(HeaderComponent);
```
- Components can be used in this way but it is not how we do it. 
- Idea is to build a tree of components with one root component.
- **Why?** Exchange data and communicate with each other.
- **Then How?** Call bootstrap only once and use other components inside app/root component.

### Important Concept Regarding CSS
- A component can only see its own styles. For example, styles defined in `header.component.css` can only be seen by `HeaderComponent` and not by `AppComponent`.

- Above point maybe easy to understand as header component is a child of app component so its style are not visible to app component.

- But, what about styles defined in `app.component.css`? Can they be seen by `HeaderComponent`? **No!** Because, Angular uses Shadow DOM</mark> to encapsulate views/styles.
- View encapsulation can be turned off using `encapsulation: ViewEncapsulation.None` in component file.

### Dynamic Content & Getters For Computed Values
- All non-private properties defined in the component class are available in template of that class.

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

// Better practice: outsource computations
<img [src]="imagePath" /> // Beware it is not a function call, it should be accessed like a property.
```
- We can omit property binding for string values and just you like react props e.g., `title="A title"`.

### State
- So easy.. no setState hanky-panky!!
- Uses zone.js behind the scene to detect changes

### Signals
- looks like retarded useState
- trackable data containers
- an object that stores a value (any type of value)
- notify Angular when values change so Angular can update parts of UI where those values are used

### Component Inputs (Props)

- Just like props of react.

```ts
export class UserComponent {
    // Receiving Props
    @Input({ required: true, alias: 'fName' }) firstName!: string // acessible as fName in template
    @Input() lastName!: string
    // ! Convinces ts that we will always have value here.

    @Input() middleName?: string
    // ? Tells ts that it might not be initialized and its ok.

    // Another alternative
    @Input() age: number | undefined
}
```
- There is another way to do this using signal inputs.


### Outputs & Emitting Data
- Reverse props?
- We can make emitters of type void like `new EventEmitter<void>()`.
```ts
@Input({ required: true }) id!: string
@Output() select = new EventEmitter() // It is better to provide type here like EventEmitter<void>, EventEmitter<string> etc.
// another way of doing this is by using output function. 
// select = output<string>()

onSelectUser() {
    this.select.emit()
    // this.select.emit('sharing a string with parent component')
}

// Event Binding -> (select), (click)
<app-user 
    (select)="onselectUser($event)"
    [id]="users[0].id"
/>
```
- There is another way to do this using `output()` function. 
- It just exists as an alternative syntax so you can write code without decorators.
- Doesn't use signals.. just a different syntax!
- But, `output()` way of doing is more secure as TypeScript throws error if no type is provided.

### For Loop

- track is like key we provide in map in react.
```ts
@for (user of users; track user.id) {
    <li>
        <app-user [user]="user" (select)="onSelectUser($event)">
    </li>
}
```

### Conditional Rendering
```ts
@if (selectedUser) {
    <app-tasks ... />
} @else {
    <p id="fallback">Any text...</p>
}
```

### Legacy: ngFor & ngIf
- These are structural directives.
- Currently, I am not learning these as new syntax is cleaner & easier.

### Storing Data Models
- Store in separate files e.g., `user.model.ts` 

### Dynamic CSS Styling with Class Bindings
```ts
// Here active is a class name defined in .css file.
<li [class.active]="selectedItem">Create Account</li>
```
### NgModel
- An **element enhancement** that helps with extracting (or changing) user input values.
- Components are directives! Directives with templates.
- **Two-way-binding:** Read & Write data from & to input field. 
- Register directive by importing `FormsModule` in imports.
- `name ` attribute is required for ngModel to work.
```ts
title = ''

<input 
    type="text"
    name="title"
    [(ngModel)]="title" // two-way-binding syntax
/>
```
- To use signals with two-way-binding, just declare state with a signal in ts file. Code in template remains same.
- If `FormsModule` is imported, Angular stops page refresh when form is submitted.
- `(ngSubmit)="onMySubmit()"` is emitted when form is submitted.

### Content Projection: Higher Order Component of React?
```html
<div>
    <!-- Acts as a placeholder for wrapped markup -->
    <ng-content /> 
</div>
```

### Transforming Template Data with Pipes
```ts
// import DatePipe & add in imports array.
{{ task.dueDate | date: 'short' }}
```

### Services
- Outsource data and logic from a component into a service and then inject that service in all the components that might be interested in the data and some of the methods exposed by the method.
- <mark>Didn't understand dependency injection here! Need a lecture on dependency injection. Maybe make another markdown file.</mark>
- 1 thing that I understood here is that if you are using `Output` too much it means you should be using services.

```ts
construction(
    private tasksService: TasksService
) {}

// Alternative Syntax
private tasksService = inject(TasksService)

@Injectable({ providedIn: 'root' })
export class TasksService {
    // ...
}
```

### Modules
- We worked with `standalone` components so far. Standalone components are recommended and most modern way of combining/making Angular components.
- Its just like modules of Nest.js. 
- **Pros:** Components decorator declaration gets leaner because we don't need imports array.
- **Cons:** We don't know which component is using which other components and we need to create extra modules.

```ts

@NgModule({
    // declare all the components/directives that need to work together
    declarations: [AppComponent, ...],
    bootstrap: [AppComponent], // Only in root module
    imports: [BrowserModule, ...], // <- Standalone components
    exports: []
})
export class AppModule {}
```
- Modules & standalone components are rivaling concepts so standalone components can't be imported into declarations rather we add them in imports.
- If we are not using root component as standalone, need to change `main.ts`.
- We can use modules in standalone components. Remember, `FormsModule`?
- `BrowserModule` already has pipes included so don't need to import them again.
- Every module must work on its own. If a module needs something, it must declare or import it itself. It can't get it from any parent module that is using this module.
- `BrowserModule` is only meant to be imported in root module. So, other modules should use `CommonModule` instead.

### Using images from public folder
- Images (and statically served assets in general) are now stored in the public/ folder - NOT in a nested assets/ folder!

- To reference images stored in the public/ folder we would use a path like this: `<img src="some-image.png">` - i.e., the public folder name is NOT part of that path (it's NOT `<img src="public/some-image.png">`)

### Pipes
- Pipes are used to transform data in the template.
- Angular provides some built-in pipes like `uppercase`, `date`, `currency`, `json`, `async` etc.

### Component tags in DOM?
- Our component elements are not compiled away and do end up the real DOM e.g., `<app-button><button>...</button></app-button>`
- Selectors found in Angular: https://angular.dev/guide/components/selectors

```ts
// Rightway
@Component({
    `selector: 'button[appButton]'` // button with attribute of appButton on them
})

// How to use?
<button appButton></button>
```


### Routing
- SPA? Single HTML file
- Give illusion/feel of multiple pages
```ts
// main.ts
bootstrapApplication(AppComponent, appConfig)

// app.config.ts
export const appConfig: ApplicationConfig = {
    providers: [
        provideRouter(routes)
    ]
}

// app.routes.ts
export const routes: Routes = [
    {
        path: 'tasks',
        component: TaskComponent 
    },
    // Dynamic Routes
    {
        path: 'users/:userId', // only :userId can be set too. We can use multiple dynamic path segments.
        component: UserTasksComponent 
    }
]

// app.component.ts
@Component({
    //....
    imports: [RouterOutlet]
})

// app.component.html
<div>
    <router-outlet>
</div>

// side-bar.component.ts
@Component({
    //....
    imports: [RouterLink, RouterLinkActive]
})

// side-bar.component.html
<a routerLink="/tasks" routerLinkActive="mySelectedClass">
    <span>{{ user().name }}</span>
</a>
// Use routerLinkActive for styling active navigation links

// user.component.ts
<a [routerLink]="['/users', user().id]">
    <span>{{ user().name }}</span>
</a>

// same as above but different syntax
<a [routerLink]="'/users' + user().id">
    <span>{{ user().name }}</span>
</a>

// user-tasks.component.ts
export class UserTasksComponent {
    // For this approach to work, we need to provide withComponentInputBinding() as second parameter to provideRouter in app.config.ts
    userId = input.required<string>() // same name as dynamic path parameter
}

// Above example uses signals, but, same can be achieved using @Input() but I won't repeat both the approaches in all places.


```

### Topics to Revisit/Revise
- Signals: Videos: [28, 32]
- Maybe some guide here after reading notes (if not enough) so if i have work with modules, this guide is still useful.

## Angular < 16

### main.ts
```ts
platformBrowserDynamic().bootstrapModule(AppModule)
```

### Data Binding
- Communication between TS Code (Business Logic) & Template (HTML)

### Directives (*ngIf, *ngFor)

- Directives are instructions in the DOM.
- Tells Angular to do something for us!

#### Structural
```ts
// * is required as its structural directive i.e., changes structure of DOM
<p *ngIf="serverCreated; else noServer">Server was created</p>
<ng-template #noServer>
    <p>No server was created!</p>
</ng-template>
```

#### Attribute
- Only change the element they are placed on.
- Don't add or remove elements.
```ts
<p 
    [ngStyle]="{ backgroundColor: 'red' }" 
    [ngClass]="{ online: serverStatus === 'online' }"
>
    {{ status }}
</p>

// let i = index is optional
<app-server *ngFor="let server of servers; let i  = index"></app-server>
```

### Local Reference in Templates
- Local reference can only be used in the template where it is defined. No in ts file.
- I think this is like `useRef` in React.
```ts
<input type="text" #serverNameInput /> // returns input element
<button (click)="onAddServer(serverNameInput)">Add Server</button>
``` 
