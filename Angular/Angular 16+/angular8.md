# Angular WebSockets, GraphQL & Accessibility (a11y)

## **📡 Real-Time Communication with WebSockets in Angular**  

### **1️⃣ Setting Up WebSockets in Angular**  
Install `socket.io-client`:  
```sh
npm install socket.io-client
```
Create a WebSocket service:  
```typescript
import { Injectable } from '@angular/core';
import { io, Socket } from 'socket.io-client';

@Injectable({ providedIn: 'root' })
export class WebSocketService {
  private socket: Socket = io('http://localhost:3000');

  sendMessage(msg: string) {
    this.socket.emit('message', msg);
  }

  receiveMessage() {
    return this.socket.fromEvent<string>('message');
  }
}
```
💡 **Why?** Enables real-time updates without polling.  

---

### **2️⃣ Using WebSockets in a Component**  
```typescript
export class ChatComponent implements OnInit {
  messages: string[] = [];
  
  constructor(private wsService: WebSocketService) {}

  ngOnInit() {
    this.wsService.receiveMessage().subscribe(msg => {
      this.messages.push(msg);
    });
  }

  sendMessage(msg: string) {
    this.wsService.sendMessage(msg);
  }
}
```
💡 **Why?** Keeps the UI reactive and responsive.  

---

## **🔗 GraphQL with Angular**  

### **1️⃣ Setting Up Apollo GraphQL**  
```sh
ng add apollo-angular
```
Create a GraphQL service:  
```typescript
import { Injectable } from '@angular/core';
import { Apollo, gql } from 'apollo-angular';

@Injectable({ providedIn: 'root' })
export class GraphqlService {
  constructor(private apollo: Apollo) {}

  getUsers() {
    return this.apollo.watchQuery({
      query: gql`{ users { id name email } }`
    }).valueChanges;
  }
}
```
💡 **Why?** Fetches data efficiently with minimal over-fetching.  

---

### **2️⃣ Executing GraphQL Mutations**  
```typescript
const ADD_USER = gql`
  mutation AddUser($name: String!, $email: String!) {
    addUser(name: $name, email: $email) {
      id
      name
    }
  }
`;

this.apollo.mutate({ mutation: ADD_USER, variables: { name: 'John', email: 'john@example.com' } });
```
💡 **Why?** Optimally modifies data without redundant API calls.  

---

## **🎨 Accessibility (a11y) Best Practices in Angular**  

### **1️⃣ Using ARIA Attributes for Accessibility**  
```html
<button aria-label="Close">✖</button>
```
💡 **Why?** Helps screen readers interpret UI elements.  

---

### **2️⃣ Ensuring Keyboard Navigation**  
```typescript
@HostListener('keydown.arrowdown', ['$event'])
onKeydown(event: KeyboardEvent) {
  // Handle arrow down navigation
}
```
💡 **Why?** Supports users who navigate using keyboards.  

---

### **3️⃣ Enabling High Contrast Mode Support**  
```css
@media (prefers-contrast: high) {
  button { background: black; color: white; }
}
```
💡 **Why?** Improves visibility for visually impaired users.  

---

## **🤖 Mock Interview Questions**  
1. How do you implement **real-time updates using WebSockets in Angular**?  
2. What are the **advantages of using GraphQL over REST in Angular**?  
3. How do you **enhance accessibility (a11y) in an Angular application**?  
4. What are **ARIA attributes, and why are they important in Angular applications**?  

---

### ✅ **Final Tips**  
- **Understand WebSockets and how to use them in Angular.**  
- **Learn GraphQL and how it improves API efficiency.**  
- **Apply accessibility best practices for a more inclusive UI.**  

Best of luck with your interview! 🚀