# Angular Interview Preparation Guide

## Core Angular Concepts
### Components, Directives, Pipes, Services, Dependency Injection
- **Components**: Building blocks of Angular applications.
- **Directives**: Attribute & structural directives (e.g., `ngIf`, `ngFor`).
- **Pipes**: Transform data in templates (`uppercase`, `date`, `custom pipes`).
- **Services & Dependency Injection**: Share logic across components.

## Advanced Angular Topics
### Change Detection
- **Default**: Checks the entire component tree.
- **OnPush**: Checks only when `@Input` changes (better performance).
- **ChangeDetectorRef**: Manually trigger change detection.

```typescript
@Component({
  selector: 'app-demo',
  template: `<p>{{ data }}</p>`,
  changeDetection: ChangeDetectionStrategy.OnPush
})
```

### State Management
#### NgRx (Redux-style State Management)
- **Store**: Holds the application state.
- **Actions**: Events that modify state.
- **Reducers**: Define how state changes.
- **Selectors**: Extract slices of state.
- **Effects**: Handle side effects (e.g., API calls).

#### Akita (Simpler alternative to NgRx)
- Stores, Queries, Entities, Actions.

#### Angular Signals (Introduced in Angular 16)
- **Signals**: Reactive variables.
- **Computed**: Derive new values.
- **Effects**: Run side effects.

#### Earlier State Management Approaches
1. Component state
2. `@Input()` & `@Output()`
3. Services with `BehaviorSubject`
4. LocalStorage / SessionStorage
5. EventEmitters

## Performance Optimization
### Lazy Loading Modules
- Load feature modules only when needed.
- Example:

```typescript
const routes: Routes = [
  { path: 'feature', loadChildren: () => import('./feature.module').then(m => m.FeatureModule) }
];
```

### Optimizing RxJS & Async Operations
- **Debounce API calls**

```typescript
this.searchBarValue.pipe(
  debounceTime(300),
  switchMap(() => this.http.get("url")),
  shareReplay(1)
).subscribe(value => this.resultValue = value);
```

- **`switchMap()`**: Cancels previous request before making a new one.
- **`tap()`**: Logs values without affecting stream.

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

## Client-Side Storage Options in Angular

| Storage Type       | Description | Data Persistence | Size Limit | Use Cases |
|--------------------|-------------|------------------|------------|-----------|
| **Cookies** 🍪  | Small key-value storage sent with HTTP requests. Can have expiration time. | Until expiration (or manually deleted) | ~4KB | Authentication tokens, user preferences |
| **Local Storage** 🗄️  | Stores data permanently in the browser until manually cleared. | Persistent | ~5-10MB | User settings, cache data, app states |
| **Session Storage** ⏳  | Stores data for a single session (cleared when the tab is closed). | Until browser tab is closed | ~5MB | Temporary form data, session-based user data |
| **Cache Storage** ⚡  | Stores network responses and assets for offline access, managed by Service Workers. | Until cache is cleared | Varies | PWA caching, faster load times, offline support |
| **IndexedDB** 🏦  | NoSQL database for storing large structured data, supports transactions. | Persistent | 50MB+ (browser-dependent) | Large-scale storage, offline databases, caching complex data |



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
- **reduce()** - Iterates over an array and returns a single accumulated value.
- **map()** - Creates a new array by transforming each element.
- **filter()** - Returns a new array with only elements that satisfy a condition.
- **find()** - Returns the first matching element based on a condition.
- **some()** - Checks if at least one element satisfies the condition. Returns a boolean.
- **every()** - Checks if all elements satisfy the condition. Returns a boolean.

### Event Loop & Async JavaScript
- **setTimeout()** - Delays execution of a function.
- **Promise** - Handles asynchronous operations.
- **async/await** - Simplifies working with Promises.

### Other JavaScript Concepts
- **Closures** - Functions remembering outer variables.
- **Hoisting** - JavaScript moves function and variable declarations to the top.
- **Prototype & Inheritance** - Objects inherit properties/methods from prototypes.
- **Currying** - Transforms a function so arguments are applied one at a time.
