# Senior Angular Developer Interview Preparation

## 📌 Overview
This document provides a structured guide to preparing for a **Senior Angular Developer** interview, especially for **Angular 18**.

---
## 🔥 Angular 18 Key Features

### 1️⃣ **Deferrable Views**
Deferrable views improve lazy loading inside templates.
```html
<ng-container *defer>
  <app-heavy-component></app-heavy-component>
</ng-container>
```
**Benefit:** Improves performance by rendering components only when needed.

### 2️⃣ **Signals API** (State Management Alternative)
```typescript
import { signal } from '@angular/core';
const count = signal(0);
count.set(count() + 1);
```
**Why?** Eliminates unnecessary re-renders, making Angular faster.

### 3️⃣ **View Transitions API**
Used for **smooth page transitions**.
```typescript
import { provideViewTransitions } from '@angular/platform-browser';
bootstrapApplication(AppComponent, {
  providers: [provideViewTransitions()]
});
```

### 4️⃣ **Standalone Components**
No need for `NgModules` anymore!
```typescript
@Component({
  selector: 'app-hello',
  standalone: true,
  template: '<h1>Hello World</h1>'
})
export class HelloComponent {}
```

### 5️⃣ **Performance Enhancements**
- Faster hydration for **SSR apps**.
- Smaller **bundle sizes**.

---
## 🚀 Core Angular Topics

### 1️⃣ **Change Detection**
```typescript
@Component({
  selector: 'app-example',
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: '{{ data }}'
})
export class ExampleComponent {
  @Input() data: string;
}
```
Use **OnPush** to optimize performance!

### 2️⃣ **Component Communication**
#### **Parent to Child**
```typescript
@Input() data: string;
```
#### **Child to Parent**
```typescript
@Output() clicked = new EventEmitter<string>();
```
#### **Service Communication**
```typescript
@Injectable({ providedIn: 'root' })
class DataService {
  private dataSubject = new BehaviorSubject<string>('Initial Value');
  data$ = this.dataSubject.asObservable();
}
```

### 3️⃣ **Routing with Lazy Loading**
```typescript
const routes: Routes = [
  { path: 'dashboard', loadComponent: () => import('./dashboard.component').then(m => m.DashboardComponent) }
];
```

### 4️⃣ **State Management (NgRx & Signals)**
```typescript
// Using Signals for state management
const userState = signal<User | null>(null);
userState.set({ name: 'John Doe' });
```

### 5️⃣ **Forms (Reactive)**
```typescript
form = new FormGroup({
  name: new FormControl('', [Validators.required])
});
```

### 6️⃣ **Testing**
```typescript
it('should call the service method', () => {
  spyOn(service, 'getData').and.returnValue(of(['Test Data']));
  component.ngOnInit();
  expect(service.getData).toHaveBeenCalled();
});
```

---
## 📦 System Design & Micro Frontends

### **How to Design a Scalable Angular App?**
- Use **Lazy Loading**
- Implement **Standalone Components**
- Use **Nx for Monorepos**
- Optimize **Change Detection**

### **Micro Frontends using Module Federation**
```typescript
// Webpack Config for Micro Frontends
new ModuleFederationPlugin({
  name: 'hostApp',
  remotes: {
    remoteApp: 'remoteApp@http://localhost:4201/remoteEntry.js'
  }
})
```