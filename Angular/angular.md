# Angular Interview Preparation Guide

## Core Angular Concepts
### Components, Directives, Pipes, Services, Dependency Injection
- **Components**: The fundamental building blocks of Angular applications, responsible for handling the UI and logic.
- **Directives**:
  - **Attribute Directives**: Modify the behavior or appearance of an element (`ngClass`, `ngStyle`).
  - **Structural Directives**: Change the DOM structure (`ngIf`, `ngFor`, `ngSwitch`).
- **Pipes**: Transform data before displaying it in a template (`uppercase`, `date`, `custom pipes`).
- **Services & Dependency Injection**: Share reusable logic across components and improve modularity and maintainability.

## Advanced Angular Topics
### Change Detection
- **Default Change Detection**: Angular checks the entire component tree for changes, triggered by events, user interactions, or API responses.
- **OnPush Strategy**: Optimizes performance by checking only when `@Input` changes or explicitly triggered using `ChangeDetectorRef`.
- **ChangeDetectorRef**: Allows manual triggering of change detection cycles to optimize rendering.

```typescript
@Component({
  selector: 'app-demo',
  template: `<p>{{ data }}</p>`,
  changeDetection: ChangeDetectionStrategy.OnPush
})
```

### State Management
#### NgRx (Redux-style State Management)
- **Store**: Central repository holding the application state.
- **Actions**: Define events that modify state.
- **Reducers**: Specify how state transitions occur.
- **Selectors**: Extract slices of state efficiently.
- **Effects**: Handle side effects like API calls asynchronously.

#### Akita (Simpler alternative to NgRx)
- Uses **Stores, Queries, Entities, Actions** for lightweight state management.

#### Angular Signals (Introduced in Angular 16)
- **Signals**: Reactive variables that respond to changes.
- **Computed**: Derived values based on signals.
- **Effects**: Run side effects when signals change.

#### Earlier State Management Approaches
1. Component state (local variables)
2. `@Input()` & `@Output()` for parent-child communication
3. Services with `BehaviorSubject` for shared state
4. LocalStorage / SessionStorage for persistence
5. EventEmitters for component communication

## Performance Optimization
### Lazy Loading Modules
- Load feature modules dynamically to improve performance.

```typescript
const routes: Routes = [
  { path: 'feature', loadChildren: () => import('./feature.module').then(m => m.FeatureModule) }
];
```

### Optimizing RxJS & Async Operations
- **Debounce API calls** to prevent excessive requests.

```typescript
this.searchBarValue.pipe(
  debounceTime(300),
  switchMap(() => this.http.get("url")),
  shareReplay(1)
).subscribe(value => this.resultValue = value);
```

- **`switchMap()`**: Cancels previous request before making a new one.
- **`tap()`**: Logs values without affecting the stream.

### `trackBy` in `ngFor`
- Improves performance by reducing DOM updates.

```typescript
<li *ngFor="let user of users; trackBy: trackById">{{ user.name }}</li>
```

### Tree Shaking
- Removes unused code from the final bundle.
- Use:
  - `npm prune`
  - `npm dedupe`

## Caching & Storage in Angular
### Types of Caching
1. **Browser Caching**: Controlled by server headers (`Cache-Control`, `ETag`).
2. **HTTP Request Caching**: Store API responses for reuse.
3. **Service Worker (PWA Caching)**: Cache assets & API responses.
4. **RxJS Caching (`shareReplay(1)`)**: Reuse API results across multiple subscribers.

### Storage Types
1. **Cookies**
2. **Local Storage**
3. **Session Storage**
4. **Cache Storage** (Service Worker controlled)
5. **IndexedDB**

## Reusability in Angular
### Content Projection (`ng-content`)
- Allows inserting content into a component dynamically.

```html
<app-card>
  <h2>Title</h2>
  <p>Description</p>
</app-card>
```

```typescript
@Component({
  selector: 'app-card',
  template: `<ng-content></ng-content>`
})
```

## JavaScript Concepts
### Array Methods
- **`reduce()`** - Iterates over an array and returns a single accumulated value.
- **`map()`** - Creates a new array by transforming each element.
- **`filter()`** - Returns a new array with only elements that satisfy a condition.
- **`find()`** - Returns the first matching element based on a condition.
- **`some()`** - Checks if at least one element satisfies the condition. Returns a boolean.
- **`every()`** - Checks if all elements satisfy the condition. Returns a boolean.

```javascript
const numbers = [1, 2, 3, 4, 5];
console.log(numbers.map(num => num * 2)); // [2, 4, 6, 8, 10]
```

### Event Loop & Async JavaScript
- **setTimeout()** - Executes code after a delay.
- **Promise** - Handles asynchronous operations.
- **async/await** - Simplifies working with Promises.

```javascript
async function fetchData() {
  const response = await fetch('https://api.example.com/data');
  const data = await response.json();
  console.log(data);
}
```

### Other JavaScript Concepts
- **Closures** - Functions that remember the variables from their outer scope.

```javascript
function outerFunction(outerVariable) {
  return function innerFunction(innerVariable) {
    console.log(`Outer: ${outerVariable}, Inner: ${innerVariable}`);
  };
}
const newFunction = outerFunction("Hello");
newFunction("World"); // Output: Outer: Hello, Inner: World
```

- **Hoisting** - JavaScript moves function and variable declarations to the top.

```javascript
console.log(name); // Undefined (due to hoisting)
var name = "John";
```

- **Prototype & Inheritance** - Objects inherit properties/methods from prototypes.

```javascript
function Person(name) {
  this.name = name;
}
Person.prototype.greet = function() {
  console.log(`Hello, my name is ${this.name}`);
};
const person1 = new Person("Alice");
person1.greet(); // Output: Hello, my name is Alice
```

- **Currying** - Transforming a function so arguments are applied one at a time.

```javascript
function add(a) {
  return function(b) {
    return a + b;
  };
}
const add5 = add(5);
console.log(add5(3)); // Output: 8
```

