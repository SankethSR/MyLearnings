# Angular 15 and Below - Interview Preparation Guide

## **🔹 Core Angular Concepts**

### **1️⃣ Modules & Components**
#### **Angular Modules (`@NgModule`)**
Angular apps are modular, and every Angular app has at least one module: `AppModule`.

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

#### **Component Lifecycle Hooks**
- `ngOnInit()`: Runs after component initialization.
- `ngOnChanges()`: Responds to `@Input()` changes.
- `ngAfterViewInit()`: Executes after view rendering.
- `ngOnDestroy()`: Used for cleanup (unsubscribe observables, remove event listeners).

```typescript
import { Component, OnInit, OnDestroy } from '@angular/core';

@Component({ selector: 'app-example', template: `<p>Example Component</p>` })
export class ExampleComponent implements OnInit, OnDestroy {
  ngOnInit() {
    console.log('Component initialized');
  }

  ngOnDestroy() {
    console.log('Component destroyed');
  }
}
```

---

## **🔹 Services & Dependency Injection**

### **2️⃣ Creating and Using a Service**

```typescript
import { Injectable } from '@angular/core';

@Injectable({ providedIn: 'root' })
export class DataService {
  getData() {
    return ['Angular', 'React', 'Vue'];
  }
}
```

Using the service in a component:
```typescript
import { Component, OnInit } from '@angular/core';
import { DataService } from './data.service';

@Component({ selector: 'app-root', template: `<ul><li *ngFor="let item of data">{{ item }}</li></ul>` })
export class AppComponent implements OnInit {
  data: string[] = [];
  constructor(private dataService: DataService) {}
  ngOnInit() {
    this.data = this.dataService.getData();
  }
}
```

---

## **🔹 Forms: Template-Driven vs. Reactive Forms**

### **3️⃣ Template-Driven Forms**
```html
<form #formRef="ngForm" (ngSubmit)="onSubmit(formRef)">
  <input name="username" ngModel required>
  <button type="submit">Submit</button>
</form>
```
```typescript
onSubmit(form: NgForm) {
  console.log(form.value);
}
```

### **4️⃣ Reactive Forms**
```typescript
import { Component } from '@angular/core';
import { FormGroup, FormControl, Validators } from '@angular/forms';

@Component({ selector: 'app-form', template: `<form [formGroup]="form" (ngSubmit)="onSubmit()">
  <input formControlName="username">
  <button type="submit">Submit</button>
</form>` })
export class FormComponent {
  form = new FormGroup({ username: new FormControl('', Validators.required) });
  onSubmit() {
    console.log(this.form.value);
  }
}
```

---

## **🔹 Performance Optimization & Best Practices**

### **5️⃣ Change Detection Strategies**
```typescript
import { ChangeDetectionStrategy, Component } from '@angular/core';

@Component({
  selector: 'app-demo',
  template: `<p>{{ data }}</p>`,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class DemoComponent {
  data = 'Hello';
}
```
💡 **Why?** `OnPush` prevents unnecessary re-rendering.

---

## **🔹 RxJS & Asynchronous Programming**

### **6️⃣ Common RxJS Operators**
```typescript
import { of } from 'rxjs';
import { map, filter } from 'rxjs/operators';

of(1, 2, 3, 4).pipe(
  filter(x => x % 2 === 0),
  map(x => x * 10)
).subscribe(console.log);
```

### **7️⃣ Handling HTTP Requests with Interceptors**
```typescript
import { HttpInterceptor, HttpRequest, HttpHandler } from '@angular/common/http';
import { Injectable } from '@angular/core';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler) {
    const modifiedReq = req.clone({ headers: req.headers.set('Authorization', 'Bearer token') });
    return next.handle(modifiedReq);
  }
}
```

---

## **🔹 Mock Interview Questions**

### **💡 Theoretical Questions**
1. What is the difference between `ViewChild` and `ContentChild`?
2. Explain Lazy Loading and why it’s important.
3. What is the difference between Pure & Impure Pipes?
4. How does Change Detection work in Angular?
5. Explain the role of `Renderer2` in Angular.

### **📝 Scenario-Based Questions**
1. How would you optimize an Angular application for performance?
2. Implement a custom directive to change text color on hover.
3. How do you handle caching API responses in Angular?
4. Create a custom form validator for validating passwords.
5. Implement an Angular route guard that prevents unauthorized access.

---

## **✅ Final Tips**
- **Understand the basics** of Components, Services, and Dependency Injection.
- **Optimize performance** using Lazy Loading, Change Detection, and RxJS best practices.
- **Know common interview topics** like Forms, Routing, and State Management.
- **Practice scenario-based coding questions** to improve problem-solving skills.

🚀 Best of luck with your interview!
