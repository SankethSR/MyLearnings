# Advanced Angular Topics

## **🛠 Web Workers in Angular**

### **1️⃣ Offloading Heavy Computations**
Web Workers allow running scripts in the background without blocking the UI.

#### **Enabling Web Workers in Angular**
```sh
ng generate web-worker my-worker
```
This creates `my-worker.worker.ts`, which runs in a separate thread.

#### **Using Web Workers**
```typescript
if (typeof Worker !== 'undefined') {
  const worker = new Worker(new URL('./my-worker.worker', import.meta.url));
  worker.postMessage('Hello');
  worker.onmessage = ({ data }) => {
    console.log(`Worker said: ${data}`);
  };
}
```
💡 **Why?** Prevents the UI from freezing during CPU-intensive operations.

---

## **📦 Monorepos with Nx**

### **1️⃣ Setting Up Nx for Angular**
```sh
npx create-nx-workspace@latest myorg
```

### **2️⃣ Creating Angular Apps in Nx**
```sh
nx g @nrwl/angular:app my-angular-app
```
💡 **Why?** Efficiently manage multiple Angular projects in a single repository.

---

## **🚀 CI/CD for Angular Apps**

### **1️⃣ GitHub Actions for Angular**
Create `.github/workflows/ci.yml`:
```yaml
name: Angular CI
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install dependencies
        run: npm install
      - name: Build Angular app
        run: npm run build
```
💡 **Why?** Automates testing and deployment, improving development efficiency.

---

## **🔄 Angular Hybrid Applications (Migration from AngularJS)**

### **1️⃣ Using ngUpgrade to Migrate**
```sh
npm install @angular/upgrade
```

Modify `main.ts`:
```typescript
import { UpgradeModule } from '@angular/upgrade/static';
platformBrowserDynamic().bootstrapModule(AppModule).then(platformRef => {
  platformRef.injector.get(UpgradeModule).bootstrap(document.body, ['legacyApp']);
});
```
💡 **Why?** Enables a step-by-step migration from AngularJS to Angular.

---

## **🔒 Security Best Practices in Angular**

### **1️⃣ Preventing XSS Attacks**
```typescript
this.sanitizedHtml = this.sanitizer.bypassSecurityTrustHtml(userInput);
```
💡 **Why?** Ensures untrusted user input doesn’t execute malicious scripts.

### **2️⃣ Implementing JWT Authentication**
```typescript
http.post('/login', { username, password }).subscribe(token => localStorage.setItem('jwt', token));
```
💡 **Why?** Secures authentication via JSON Web Tokens (JWTs).

---

## **⚡ WebAssembly (WASM) with Angular**

### **1️⃣ Running High-Performance Code with WASM**
```html
<script>
  fetch('app.wasm').then(response => response.arrayBuffer()).then(bytes => WebAssembly.instantiate(bytes));
</script>
```
💡 **Why?** Improves performance for computationally expensive tasks.

---

## **🤖 AI-Powered Applications in Angular**

### **1️⃣ Integrating TensorFlow.js**
```typescript
import * as tf from '@tensorflow/tfjs';
const model = await tf.loadLayersModel('model.json');
const prediction = model.predict(tf.tensor([input]));
```
💡 **Why?** Enables machine learning in Angular applications.

### **2️⃣ Using OpenAI API for Chatbots**
```typescript
fetch('https://api.openai.com/v1/chat/completions', {
  method: 'POST',
  headers: { 'Authorization': `Bearer YOUR_API_KEY` },
  body: JSON.stringify({ model: 'gpt-4', messages: [{ role: 'user', content: 'Hello' }] })
});
```
💡 **Why?** Integrates AI-powered responses into Angular applications.

---

## **🤖 Mock Interview Questions**
1. How do **Web Workers** improve performance in Angular?
2. What are the benefits of **Nx monorepos** for Angular applications?
3. How would you set up **CI/CD for an Angular app**?
4. How does **ngUpgrade** help migrate an AngularJS app?
5. What are the best practices to **secure an Angular application**?
6. How does **WebAssembly (WASM)** enhance performance in Angular?
7. How can AI be integrated into **Angular applications**?

---

### ✅ **Final Tips**
- **Master performance optimizations using Web Workers and WASM.**
- **Implement CI/CD workflows for smooth deployments.**
- **Ensure application security using best practices.**
- **Explore AI-powered solutions for an enhanced user experience.**

🚀 Best of luck with your interview!