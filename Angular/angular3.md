# Advanced Angular 18 Interview Preparation

## **🔥 Advanced Angular Concepts**

### **1️⃣ Signals vs RxJS Observables**
| Feature  | Signals  | Observables |
|----------|---------|------------|
| **Push-based** | Yes | Yes |
| **Pull-based** | Yes | No |
| **Sync Updates** | Yes | No (Async) |
| **Optimized Change Detection** | Yes | No |
| **RxJS Compatibility** | Partial | Yes |

💡 **When to Use Signals?**
- When you need **fine-grained reactivity** without extra boilerplate.
- When **state management** should be simple and synchronous.

💡 **When to Use Observables?**
- When dealing with **asynchronous streams** (HTTP, WebSockets, Events).

---

### **2️⃣ Custom Directives**
```typescript
@Directive({ selector: '[appHighlight]' })
export class HighlightDirective {
  constructor(private el: ElementRef) {
    this.el.nativeElement.style.backgroundColor = 'yellow';
  }
}
```
💡 **Why Use Directives?**
- Custom behavior for elements
- Reusable UI logic

---

### **3️⃣ Dependency Injection (DI) & Providers**
#### **Multi-Providers Example**
```typescript
const MY_TOKEN = new InjectionToken<string[]>('MY_TOKEN');

@NgModule({
  providers: [
    { provide: MY_TOKEN, useValue: ['Item1'], multi: true },
    { provide: MY_TOKEN, useValue: ['Item2'], multi: true }
  ]
})
```
💡 **Why Multi-Providers?**
- Useful when multiple services need to be injected dynamically.

---

## **🚀 Performance Optimization**

### **1️⃣ Avoid Unnecessary Change Detection**
- Use `ChangeDetectionStrategy.OnPush`
- Prefer **Signals** over `BehaviorSubject`
- Use `trackBy` in `*ngFor`

```html
<li *ngFor="let user of users; trackBy: trackById">{{ user.name }}</li>
```
```typescript
trackById(index: number, user: User) {
  return user.id;
}
```

---

### **2️⃣ Web Workers for Heavy Computation**
```typescript
const worker = new Worker(new URL('./app.worker', import.meta.url));
worker.postMessage(data);
worker.onmessage = (event) => {
  console.log('Worker Response:', event.data);
};
```
💡 **Use Case:** Offload expensive computations to a separate thread.

---

## **💻 Real-world Coding Scenarios**

### **1️⃣ Implement a Debounced Search Input**
```typescript
searchTerm = new FormControl();
ngOnInit() {
  this.searchTerm.valueChanges.pipe(
    debounceTime(300),
    distinctUntilChanged(),
    switchMap(term => this.searchService.search(term))
  ).subscribe(results => this.results = results);
}
```

---

### **2️⃣ Build a Custom Async Pipe**
```typescript
@Pipe({ name: 'customAsync', pure: false })
export class CustomAsyncPipe implements PipeTransform {
  private latestValue: any;
  private subscription: Subscription;

  transform(obs$: Observable<any>): any {
    if (!this.subscription) {
      this.subscription = obs$.subscribe(value => this.latestValue = value);
    }
    return this.latestValue;
  }
}
```
💡 **Why?** Sometimes, we need more control than the built-in `async` pipe.

---

## **🤖 Mock Interview Questions**

### **Conceptual**
1. How does **Change Detection** work in Angular?
2. What is **Zone.js**, and why is it needed?
3. How do **ViewChild & ContentChild** differ?
4. Explain the **hydration process in SSR**.

### **Practical**
1. Implement a **dynamic form** with Reactive Forms.
2. Optimize an **ngFor loop** for performance.
3. Convert an existing **NgModule-based app to Standalone Components**.
4. How would you handle **multi-tenant theming** in an Angular app?

---

### ✅ **Final Tips**
- **Practice coding:** Implement a CRUD app with the latest Angular features.
- **Understand performance optimizations.**
- **Be ready for system design discussions.**
- **Mock interviews can help.**

Best of luck with your interview! 🚀
