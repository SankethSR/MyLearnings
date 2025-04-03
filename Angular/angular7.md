# Angular PWA, Server-Side Rendering & Micro Frontends

## **🌐 Progressive Web Apps (PWA) in Angular**  

### **1️⃣ Enabling PWA in Angular**  
Install the PWA package:  
```sh
ng add @angular/pwa
```
💡 **Why?** Converts an Angular app into a PWA with offline capabilities.  

---

### **2️⃣ Implementing a Service Worker**  
Modify `ngsw-config.json`:  
```json
"assetGroups": [
  {
    "name": "app",
    "installMode": "prefetch",
    "updateMode": "prefetch",
    "resources": {
      "files": ["/*.html", "/*.css", "/*.js"]
    }
  }
]
```
💡 **Why?** Enables caching and offline functionality.  

---

### **3️⃣ Adding Push Notifications**  
```typescript
this.swPush.requestSubscription({ serverPublicKey: 'YOUR_PUBLIC_KEY' })
  .then(sub => console.log('Subscribed:', sub))
  .catch(err => console.error('Subscription failed', err));
```
💡 **Why?** Engages users with real-time updates.  

---

## **🚀 Server-Side Rendering (SSR) with Angular Universal**  

### **1️⃣ Setting Up Angular Universal**  
```sh
ng add @nguniversal/express-engine
```
💡 **Why?** Improves SEO and initial load time.  

---

### **2️⃣ Updating the Server File**  
Modify `server.ts`:  
```typescript
import 'zone.js/node';
import express from 'express';

const app = express();
app.get('*', (req, res) => {
  res.sendFile(join(DIST_FOLDER, 'browser', 'index.html'));
});
```
💡 **Why?** Renders pages on the server before sending them to the client.  

---

## **🧩 Micro Frontends in Angular**  

### **1️⃣ Implementing Module Federation**  
Add the following in `webpack.config.js`:  
```javascript
new ModuleFederationPlugin({
  name: 'app1',
  remotes: {
    remoteApp: 'remoteApp@http://localhost:4201/remoteEntry.js'
  }
})
```
💡 **Why?** Enables multiple teams to work on independent frontends.  

---

### **2️⃣ Lazy Loading Remote Modules**  
```typescript
loadRemoteModule({
  type: 'module',
  remoteEntry: 'http://localhost:4201/remoteEntry.js',
  exposedModule: './Component'
}).then(m => m.RemoteComponent);
```
💡 **Why?** Loads micro frontends dynamically.  

---

## **🤖 Mock Interview Questions**  
1. What are the key benefits of **Progressive Web Apps**?  
2. How does **Angular Universal improve SEO and performance**?  
3. Explain **how Module Federation enables Micro Frontends**.  
4. How do you handle **lazy loading in a Micro Frontend architecture**?  

---

### ✅ **Final Tips**  
- **Understand PWA features like service workers and push notifications.**  
- **Be comfortable setting up and configuring Angular Universal.**  
- **Learn Module Federation for micro frontends.**  

Best of luck with your interview! 🚀