# Angular Performance Optimization, Testing & Security Best Practices

## **🚀 Angular Performance Optimization**

### **1️⃣ Optimizing Change Detection**  
#### **Use OnPush Change Detection**  
```typescript
@Component({
  selector: 'app-optimized',
  templateUrl: './optimized.component.html',
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class OptimizedComponent {
  @Input() data!: string;
}
```
💡 **Why?** Reduces unnecessary re-renders, making the app faster.  

---

### **2️⃣ Lazy Loading & Route Preloading**  
**Lazy Loading Example:**  
```typescript
const routes: Routes = [
  { path: 'dashboard', loadChildren: () => import('./dashboard.module').then(m => m.DashboardModule) }
];
```

**Preloading Modules for Faster Navigation:**  
```typescript
@NgModule({
  imports: [RouterModule.forRoot(routes, { preloadingStrategy: PreloadAllModules })]
})
```
💡 **Why?** Loads necessary modules only when needed.  

---

### **3️⃣ Virtual Scrolling for Large Lists**  
```html
<cdk-virtual-scroll-viewport itemSize="50" class="example-viewport">
  <div *cdkVirtualFor="let item of items">{{ item }}</div>
</cdk-virtual-scroll-viewport>
```
💡 **Why?** Improves performance by rendering only visible items.  

---

## **🛠️ Testing Strategies in Angular**

### **1️⃣ Unit Testing Components & Services**  
```typescript
describe('MyComponent', () => {
  let component: MyComponent;
  let fixture: ComponentFixture<MyComponent>;

  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [MyComponent],
      providers: [MyService]
    }).compileComponents();

    fixture = TestBed.createComponent(MyComponent);
    component = fixture.componentInstance;
  });

  it('should create the component', () => {
    expect(component).toBeTruthy();
  });
});
```
💡 **Why?** Ensures component logic works correctly.  

---

### **2️⃣ Mocking HTTP Requests with HttpTestingController**  
```typescript
it('should fetch data', () => {
  const testData = { name: 'John Doe' };

  service.getData().subscribe(data => {
    expect(data).toEqual(testData);
  });

  const req = httpMock.expectOne('api/data');
  req.flush(testData);
});
```
💡 **Why?** Simulates API responses in tests.  

---

### **3️⃣ End-to-End (E2E) Testing with Cypress**  
```javascript
describe('Login Page', () => {
  it('should log in the user', () => {
    cy.visit('/login');
    cy.get('input[name="email"]').type('test@example.com');
    cy.get('input[name="password"]').type('password');
    cy.get('button[type="submit"]').click();
    cy.url().should('include', '/dashboard');
  });
});
```
💡 **Why?** Validates user flows and UI functionality.  

---

## **🛡️ Security Best Practices**  

### **1️⃣ Prevent XSS Attacks with Safe HTML**  
```typescript
this.sanitizedHtml = this.sanitizer.bypassSecurityTrustHtml(unsafeHtml);
```
💡 **Why?** Ensures HTML is sanitized before rendering.  

---

### **2️⃣ Secure API Calls with HTTP Interceptors**  
```typescript
@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const cloned = req.clone({ setHeaders: { Authorization: `Bearer ${this.authService.getToken()}` } });
    return next.handle(cloned);
  }
}
```
💡 **Why?** Automatically attaches auth tokens to requests.  

---

### **3️⃣ Implement Content Security Policy (CSP)**  
```html
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self' 'nonce-abc123';">
```
💡 **Why?** Prevents loading malicious scripts.  

---

## **🤖 Mock Interview Questions**  
1. How do you **optimize an Angular app’s performance**?  
2. How do you handle **security vulnerabilities** in Angular?  
3. What are the **best practices for testing Angular applications**?  
4. How do you implement **lazy loading and preloading strategies**?  

---

### ✅ **Final Tips**  
- **Focus on performance optimizations using ChangeDetectionStrategy & Lazy Loading.**  
- **Practice writing unit tests and API mocking techniques.**  
- **Understand security principles like CSP & XSS prevention.**  

Best of luck with your interview! 🚀
