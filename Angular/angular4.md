# System Design & Micro Frontends in Angular 18

## **🛠️ System Design for Scalable Angular Applications**

### **1️⃣ Monolithic vs. Micro Frontend Architecture**
| Feature            | Monolithic Angular | Micro Frontend |
|-------------------|------------------|---------------|
| **Deployment**    | Single build & deploy | Independent deploys |
| **Codebase**     | Single repository | Multiple repositories |
| **Performance**   | Larger bundle size | Optimized per micro app |
| **Flexibility**   | Hard to update modules independently | Independent teams can work on different MFEs |

💡 **When to use Micro Frontends?**  
- Large teams working on separate features  
- Need for independent deployments  
- Reducing initial load time by loading only necessary modules  

---

### **2️⃣ Angular Monorepo with Nx**  
- Organize large projects into **multiple apps and shared libraries**  
- Enforces **modular architecture**  
- Uses **caching to speed up builds**  

Example `nx.json`:  
```json
{
  "npmScope": "myorg",
  "affected": { "defaultBase": "main" }
}
```

💡 **Best Practice**:  
- Use **libraries** (`@nrwl/angular:library`) for **shared components/services**  
- Define **clear boundaries** to avoid dependencies between unrelated modules  

---

### **3️⃣ Micro Frontends with Module Federation**  
**Host (Shell) Application**:  
```typescript
new ModuleFederationPlugin({
  name: 'hostApp',
  remotes: {
    remoteApp: 'remoteApp@http://localhost:4201/remoteEntry.js'
  }
});
```

**Remote Application:**  
```typescript
new ModuleFederationPlugin({
  name: 'remoteApp',
  filename: 'remoteEntry.js',
  exposes: {
    './Component': './src/app/remote.component.ts',
  }
});
```

💡 **Challenges in Micro Frontends**:  
- **State Management** → Use shared **RxJS Store or Signals**  
- **Communication between Micro Frontends** → Use **Custom Events** or **Shared Services**  

---

## **📦 Real-World Architecture Scenarios**

### **1️⃣ Multi-Tenant Theming in Angular**  
- Store **themes in a JSON config**  
- Use `CSS Variables` to dynamically switch styles  

```css
:root {
  --primary-color: red;
}

[data-theme='dark'] {
  --primary-color: black;
}
```
```typescript
document.documentElement.setAttribute('data-theme', 'dark');
```

---

### **2️⃣ Authentication Best Practices**  
- Use **JWT + Refresh Tokens**  
- Secure APIs with **OAuth2**  
- Implement **Role-Based Access Control (RBAC)**  

Example Angular `AuthGuard`:  
```typescript
@Injectable({ providedIn: 'root' })
export class AuthGuard implements CanActivate {
  constructor(private authService: AuthService) {}
  canActivate(): boolean {
    return this.authService.isAuthenticated();
  }
}
```

---

### **3️⃣ Error Handling and Logging**  
Use **Global Error Handler**:  
```typescript
@Injectable()
export class GlobalErrorHandler implements ErrorHandler {
  handleError(error: any) {
    console.error('Error Occurred:', error);
  }
}
```

Use **Sentry or LogRocket** for error tracking.  

---

## **🤖 Mock System Design Questions**  
1. How would you **design a scalable Angular application**?  
2. How do you handle **inter-microfrontend communication**?  
3. How would you implement **feature flags** in Angular?  
4. How do you ensure **high performance in a large Angular app**?  

---

### ✅ **Final Tips**  
- **Prepare for system design discussions.**  
- **Practice implementing a micro frontend architecture.**  
- **Optimize Angular performance using best practices.**  

Best of luck with your interview! 🚀