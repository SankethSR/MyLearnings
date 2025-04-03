# Angular State Management, Web Performance & Advanced Concepts

## **🔄 State Management in Angular**  

### **1️⃣ Choosing a State Management Approach**  
| **Approach**  | **Use Case**  | **Example Library**  |
|--------------|--------------|------------------|
| Local State  | Component-specific state  | `@Input()`, `@Output()`, Signals  |
| Service-Based State  | Shared state between components  | `BehaviorSubject`, `RxJS`  |
| Global State  | Large applications with multiple modules  | `NgRx`, `Akita`, `Zustand`  |

---

### **2️⃣ Using RxJS for State Management**  
```typescript
@Injectable({ providedIn: 'root' })
export class UserService {
  private userSubject = new BehaviorSubject<User | null>(null);
  user$ = this.userSubject.asObservable();

  setUser(user: User) {
    this.userSubject.next(user);
  }
}
```
💡 **Why?** Keeps the application reactive and state-driven.  

---

### **3️⃣ Managing State with NgRx**  
```typescript
export const userReducer = createReducer(
  initialState,
  on(loginSuccess, (state, { user }) => ({ ...state, user }))
);
```
💡 **Why?** Provides a structured way to manage complex app state.  

---

## **⚡ Web Performance Best Practices**  

### **1️⃣ Code Splitting & Lazy Loading**  
```typescript
const routes: Routes = [
  { path: 'dashboard', loadChildren: () => import('./dashboard/dashboard.module').then(m => m.DashboardModule) }
];
```
💡 **Why?** Reduces the initial bundle size for faster load times.  

---

### **2️⃣ Tree Shaking & Bundle Optimization**  
- Use **ES Modules** (`import { Feature } from '@angular/core'`)
- Remove unused code with **Terser & Webpack**
- Analyze bundle size with  
```sh
ng build --stats-json && webpack-bundle-analyzer dist/stats.json
```
💡 **Why?** Eliminates dead code, reducing payload size.  

---

### **3️⃣ Optimizing Images & Lazy Loading Assets**  
```html
<img src="image.jpg" loading="lazy" alt="Optimized Image">
```
💡 **Why?** Prevents unnecessary resource loading and improves performance.  

---

## **🛠️ Advanced Angular Concepts**  

### **1️⃣ Custom Directives**  
```typescript
@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(el: ElementRef) {
    el.nativeElement.style.backgroundColor = 'yellow';
  }
}
```
💡 **Why?** Enhances reusability by extending element behavior.  

---

### **2️⃣ Angular Signals for Reactive UI**  
```typescript
const counter = signal(0);

function increment() {
  counter.set(counter() + 1);
}
```
💡 **Why?** Provides an optimized, reactive alternative to traditional RxJS Observables.  

---

### **3️⃣ Hybrid Angular + Web Components**  
```typescript
createCustomElement(MyComponent, { injector: this.injector });
```
💡 **Why?** Enables cross-framework compatibility.  

---

## **🤖 Mock Interview Questions**  
1. How do you manage **state efficiently in an Angular application**?  
2. What are the **best strategies for improving Angular app performance**?  
3. Explain the differences between **RxJS and Angular Signals**.  
4. How do you create **custom directives in Angular**?  

---

### ✅ **Final Tips**  
- **Understand different state management approaches and when to use them.**  
- **Optimize performance using lazy loading, tree shaking, and bundle analysis.**  
- **Master advanced Angular concepts like directives, signals, and web components.**  

Best of luck with your interview! 🚀
