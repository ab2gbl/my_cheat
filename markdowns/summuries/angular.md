
# 🅰️ Angular Cheat Sheet

# 📦 1. Project Setup & CLI Commands
- `ng --version` for version of ng
- `ng new [project_name]` for new project
- `ng serve` run server
- `ng generate component` to generate a component
- `ng g service services/todos` to generate service
-  `ng g directive ../directives/[name]` to generate directive
- `ng g pipe ../pipes/[name]` to generate pipe



# 🧱 2. Components
> `ng generate component` to make a component
- **4 files**: `.css`, `.html`, `.spec.ts` (test), and `.ts` (component)

## 🧩 Component Structure (HTML, CSS, TS, spec)
```javascript
import { Component } from  '@angular/core';
import { RouterOutlet } from  '@angular/router';
import { Comp } from  './comp/comp'; // Importing the Title component

@Component({
	selector:  'app-root', // The selector (name of the component used in HTML)
	imports: [RouterOutlet, Comp], // Importing necessary components
	templateUrl:  './app.html', // or template: '<h1>hello world</h1>'
	styleUrls: ['./app.css'], 
})

export  class  App { //exported name
	protected  title  =  'ang60'; // variable {{title}} in the template
}
```

## 🔁 `@Input()` – Pass Data from Parent to Child Component

for this we should use `input` in **child component**

```typescript
export  class  Child { 
	parentVariable  =  input(); // Using input for parent variable
	// or
	parentVariable  =  input.required<string>(); // parent variable with required type
}
```
then on **Parent HTML** we should use :
```html
<app-title  parentVariable="Hello from Parent"  />
<!-- or to pass a variable -->
<app-title  [parentVariable]="Variable"  />
 ```
## 🔁 `@Output()` – Pass Data from Child to Parent

Use `@Output()` + `EventEmitter` in a **child component** to notify the **parent** of actions.
```typescript
@Output() delete = new EventEmitter<number>();

deleteTodo(id: number) {
  this.delete.emit(id);
}
```
Then in the parent HTML:
```html
<app-todo-item (delete)="handleDelete($event)" />
```
## 🔄 Lifecycle

[image on pictures]

# 📶 3. Variables: Signals, Computed & Effects (Modern Angular)
```javascript
export  class  App { 
	title  =  'title'; // old way
	title  =  signal('Angular 60'); // signal for reactive variable  
	// {{ title() }} to use it in html
}
```
## variable update
```javascript
value  =  signal(0);
// Increment the count by 1.
value.update(v => v + 1);
// or using set
value.set(1)
```
## Computed signal
signals that are read only and change with other signal update
```typescript
const count: WritableSignal<number> = signal(0);
const doubleCount: Signal<number> = computed(() => count() * 2);
```
## Effects
An **effect** is an operation that runs whenever one or more signal values change.
```typescript
effect(() => {
	console.log(`The current count is: ${count()}`);
	// untracked : dont run when the untracked variable run
	console.log(`User set to ${currentUser()} and the counter is ${untracked(counter)}`);
});
```
### 3 ways to use effect:

```typescript
export class Component {
	...
	// inside constructor
	constructor() {    // Register a new effect.    
		effect(() => {...});  
	}
	// assign the effect to a field
	private log = effect(() => {...});  
	// pass an `Injector` to `effect`
	initializeLogging(): void {    
		effect(() => {...}, {injector: this.injector});  
	}
}
```
# 📍 4. Event listeners 
 simply we use `(event)="functionToDo()"` 
 ```html
 <input  (keypress)="keyPressHandler()"  />
 ```
 and we put the function on the app
 ```javascript
 export  class  App {
	keyPressHandler() {
			console.log('Key pressed!');
	}
}
 ```
 ### add param
 by add `$event`
 ```html
 <input  (keypress)="keyPressHandler($event)"  />
```
```javascript
 keyPressHandler(event:  KeyboardEvent) {
	console.log('Key ', event.key, ' pressed!');
}
```

# 🌐 5. Routing & Navigation
We should add to **`app.html`** 
``` html
<!-- imports: [RouterOutlet]-->
<router-outlet  />
``` 

and on **`app.routes.ts`**
```javascript
export  const  routes:  Routes  = [
	// home path
	{
		path:  '', 
		pathMatch:  'full', // Default route
		loadComponent() {
			return  import('./home/home').then((m) =>  m.Home);
		},
	},
	// todos path
	{
		path:  'todos', 
		loadComponent() {
			return  import('./todos/todos').then((m) =>  m.Todos);
		},
	},
];
```
## 🔗 Navigation

``` html
<!-- imports: [RouterLink]-->
<nav>
	<a routerLink="/">Home</a>
	<a routerLink="/todos">Todos</a>
</nav>
<router-outlet  />
``` 

# 🧾 6. TypeScript Models

make file `models/todos.type.ts`
```typescript
//models/todos.type.ts
export  type  Todos  = {
	id:  number;
	userId:  number;
	title:  string;
	completed:  boolean;
};
```


# 🛠️ 7. Services & Dependency Injection
>  `ng g service services/todos` to generate service

For **Logic** (no UI), like **Encapsulating the data** and **HTTP Requests** 
- test file + service file
```javascript
// Todos Service
import { Injectable } from  '@angular/core';
@Injectable()
export  class  TodoService {}
```
then we can inject ( add ) it to some component using:
```javascript
// Todos Component
@Component({
	...
	providers: [TodoService],
})
export  class  TodosComponent {
}
``` 
or in all components by add the next to the service:
```javascript
// Todos Service
@Injectable(
	{
		providedIn:  'root',
	}
)
export  class  TodoService {}
```

> ## after creating model we can back to our service

```typescript
@Injectable()
export  class  TodoService {
	// array of objects of type Todo
	todoItems  =  signal<Todo[]>([
		{ id:  1, userId:  1, title:  'Learn Angular', completed:  false },
		{ id:  2, userId:  1, title:  'Build an app', completed:  false },
		{ id:  3, userId:  2, title:  'Write tests', completed:  true },
	]);
}
```
now we can use this data in our component using 
``` typescript
export  class  TodosComponent{
	todosService  =  inject(TodoService);
	todos  =  computed(() =>  this.todosService.todoItems()); // todos automatically updates when todosService changes
}
// on HTML: {{ todos()[0].title }}
```
# 🌐 8. HTTP Requests
on `app.config.ts` 
```Typescript
// app.config.ts
provideHttpClient(withFetch()), // without param it use XML
```
now we can make function on **service**
```typescript
// TodoService
export  class  TodoService {
	http  =  inject(HttpClient);
	fetchTodos() {
		const  URL  =  'https://jsonplaceholder.typicode.com/todos';
		return  this.http.get<Todo[]>(URL).pipe(
			catchError((error) => {
				console.error('Error fetching todos:', error);
				throw  error;
			})
		);
	}
}
```
then we can use the data on **component**:
```typescript
// TodosComponent
export  class  Todos  implements  OnInit {
	todosService  =  inject(TodoService);
	todosItems  =  signal<Todo[]>([]);
	// u can check OnInit() on the title "Other things"
	ngOnInit():  void {  
		this.todosService.fetchTodos().subscribe((todos) => {
		this.todosItems.set(todos);
	});
}
}
``` 
# 🧲 9. Directives
```bash
ng g directive ../directives/[name]
```
Directives are classes that add additional behavior to elements in your Angular applications.
to **modify html**, 3 types:

| Type       | Use For                             | Syntax Example            |
| ---------- | ----------------------------------- | ------------------------- |
| **Component**  | UI + behavior                       | `<app-title></app-title>` |
| **Structural** | Show/hide, loop, conditional layout | `*ngIf`, `*ngFor`         |
| **Attribute**  | Style, event, DOM behavior          | `[ngClass]`, custom ones  |

### Most common attribute directive
Common directives | Details
|--|--|
[`NgClass`](https://angular.dev/guide/directives#adding-and-removing-classes-with-ngclass) | Adds and removes a set of CSS classes.
[`NgStyle`](https://angular.dev/guide/directives#setting-inline-styles-with-ngstyle) | Adds and removes a set of HTML styles.
[`NgModel`](https://angular.dev/guide/forms/template-driven-forms)| Adds two-way data binding to an HTML form element.

### example of using directive: line-throught text
```typescript
import { Directive, ElementRef, inject, effect, input } from  '@angular/core';
@Directive({
	selector:  '[appHighlightCompletedTodos]',
})
export  class  HighlightCompletedTodos {
	isCompleted  =  input<boolean>(false);
	element  =  inject(ElementRef);
	style  =  effect(() => {
		if (this.isCompleted()) {
			this.element.nativeElement.style.textDecoration = 'line-through';
		} else {
			this.element.nativeElement.style.textDecoration =  'none';
		}
	});
}
```
and then after **importing** the **directive** on the **Component**, we can use it on **html**:
```html
@for (todo of todosItems(); track todo.id)
	<li  appHighlightCompletedTodos  [isCompleted]="todo.completed">
		{{ todo.title }}
	</li>
}
```
# 🔁 10. Pipes
```bash
ng g pipe ../pipes/[name]
```
Pipes are **functions you use in templates** to format or transform values **without changing the data**.
> search by example
```typescript
import { Pipe, PipeTransform } from  '@angular/core';
import { Todo } from  '../models/todos.type';

@Pipe({
	name:  'filterTodos',
})
export  class  FilterTodosPipe  implements  PipeTransform {
	transform(todos:  Todo[], searchBy:  string):  Todo[] {
		if (!searchBy) {
			return  todos;
		}
		const  searchTerm  =  searchBy.toLowerCase();
		return  todos.filter((todo) =>
			todo.title.toLowerCase().includes(searchTerm)
		);
	}
}
```

then we can use it on **html** using:
```javascript
// using the past pipe where 
// 		todosItems is first param
//		searchBy is second param 
todosItems() | filterTodos:searchBy()
```


# 🧩 11. NgModules  -v14)

An `NgModule` is a special class that tells Angular how to **group, configure, and compile** parts of your app.
```typescript
// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

@NgModule({
  declarations: [AppComponent, CounterComponent], // components, directives, pipes
  imports: [BrowserModule], // other modules (built-in or custom)
  providers: [], // Services/injectables available in this module
  exports: [AdminDashboardComponent], // What this module makes available to other modules
  bootstrap: [AppComponent], // root component to start app
})
export class AppModule {}
```
> **Note**: Angular v14+, you can create **standalone components** that don’t need to be declared in a module: `
  standalone: true`


# 📋 12. Reactive Forms (Better than template-driven)

More scalable for complex forms with validation.
```typescript
@Component({
...
imports: [ReactiveFormsModule],
}
export  class  Component {
	form = new FormGroup({
	  title: new FormControl('', Validators.required)
	});
	onSubmit(){}
}
```
Access input:
```html
<form  [formGroup]="loginForm"  (ngSubmit)="onSubmit()">
	<input formControlName="username" ... />
	<button  type="submit"  [disabled]="!form.valid">Submit</button>
</form>
```

# 🌀 13. RxJS Observables & Subjects in Angular
## 🔍 **What is an Observable?**

An **Observable** is a stream of data that you can **subscribe** to. It’s used for handling **async operations**, like:
-   HTTP requests
-   User input    
-   Route changes
-   Event streams
```ts
myObservable.subscribe(value => {
  console.log('Received:', value);
});
```
## 🔄 **What is `.subscribe()`?**
basically , it starts when the Observable ( data ) is ready: 
-   Starts the Observable
-   Listens for incoming data
-   Handles emitted values, errors, and completion
```ts
observable.subscribe({
  next: val => console.log(val),
  error: err => console.error(err),
  complete: () => console.log('Done')
});
```

## 📦 RxJS Subjects
Subjects are **both an Observable and an Observer** (they can emit & listen).
### 🧠 **ReplaySubject**
```ts
const subject = new ReplaySubject(1); // keeps last 1 value
subject.next('Hello');
subject.subscribe(val => console.log(val)); // prints 'Hello'
```
#### 🔹 Key Points:
-   Remembers **last N values** and replays them to **new subscribers**
-   No initial value required 
-   Useful when late subscribers need to see recent emissions
 
### 💡 **BehaviorSubject**
```typescript
const subject = new BehaviorSubject<number>(0); // initial value
subject.subscribe(val => console.log(val)); // immediately prints 0
subject.next(42); // prints 42
```   
#### 🔹 Key Points:

-   Always holds the **current/latest value**
-   **Requires an initial value**
-   Emits latest value **immediately** on subscription
    

----------

### 🆚 `ReplaySubject` vs `BehaviorSubject`

Feature | `BehaviorSubject` | `ReplaySubject(1)`
|--|--|--|
Requires initial value | ✅ Yes | ❌ No 
Stores latest value | ✅ Yes | ✅ (last N values)
Emits current on subscribe | ✅ Yes | ✅ (if emitted before) 
Use case | State, forms, UI data | Event history, caching
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzMjc4NDQ4NTddfQ==
-->