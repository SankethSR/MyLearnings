# Authentication in Angular

## Overview
Authentication in Angular can be implemented using JWT (JSON Web Token), OAuth, or session-based authentication. This guide focuses on JWT-based authentication with Angular services, guards, and interceptors.

## 1. Setting Up Authentication Service
The authentication service handles user login, logout, and token management.

### Install Dependencies
```sh
npm install @angular/common @angular/router @auth0/angular-jwt
```

### Create `auth.service.ts`
```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { BehaviorSubject, Observable } from 'rxjs';
import { map } from 'rxjs/operators';

@Injectable({ providedIn: 'root' })
export class AuthService {
  private authUrl = 'https://api.example.com/auth';
  private currentUserSubject = new BehaviorSubject<any>(null);
  public currentUser = this.currentUserSubject.asObservable();

  constructor(private http: HttpClient) {
    this.loadUser();
  }

  login(credentials: { email: string; password: string }): Observable<any> {
    return this.http.post<{ token: string }>(`${this.authUrl}/login`, credentials)
      .pipe(map(response => {
        localStorage.setItem('token', response.token);
        this.loadUser();
        return response;
      }));
  }

  logout(): void {
    localStorage.removeItem('token');
    this.currentUserSubject.next(null);
  }

  getToken(): string | null {
    return localStorage.getItem('token');
  }

  isAuthenticated(): boolean {
    return !!this.getToken();
  }

  private loadUser(): void {
    const token = this.getToken();
    if (token) {
      this.currentUserSubject.next(token); // In a real-world app, decode token to get user info
    }
  }
}
```

## 2. Protecting Routes with Auth Guards
Create an Auth Guard to prevent unauthorized users from accessing certain routes.

### Create `auth.guard.ts`
```typescript
import { Injectable } from '@angular/core';
import { CanActivate, Router } from '@angular/router';
import { AuthService } from './auth.service';

@Injectable({ providedIn: 'root' })
export class AuthGuard implements CanActivate {
  constructor(private authService: AuthService, private router: Router) {}

  canActivate(): boolean {
    if (this.authService.isAuthenticated()) {
      return true;
    }
    this.router.navigate(['/login']);
    return false;
  }
}
```

### Apply Guard in Routes
```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { AuthGuard } from './auth.guard';
import { DashboardComponent } from './dashboard.component';
import { LoginComponent } from './login.component';

const routes: Routes = [
  { path: 'dashboard', component: DashboardComponent, canActivate: [AuthGuard] },
  { path: 'login', component: LoginComponent },
  { path: '**', redirectTo: 'login' }
];

@NgModule({ imports: [RouterModule.forRoot(routes)], exports: [RouterModule] })
export class AppRoutingModule {}
```

## 3. Intercepting Requests with HTTP Interceptors
Interceptors automatically attach JWT tokens to outgoing requests.

### Create `auth.interceptor.ts`
```typescript
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler } from '@angular/common/http';
import { AuthService } from './auth.service';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  constructor(private authService: AuthService) {}

  intercept(req: HttpRequest<any>, next: HttpHandler) {
    const token = this.authService.getToken();
    if (token) {
      req = req.clone({ setHeaders: { Authorization: `Bearer ${token}` } });
    }
    return next.handle(req);
  }
}
```

### Register the Interceptor
```typescript
import { NgModule } from '@angular/core';
import { HTTP_INTERCEPTORS } from '@angular/common/http';
import { AuthInterceptor } from './auth.interceptor';

@NgModule({
  providers: [{ provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true }]
})
export class AppModule {}
```

## 4. Creating a Login Component
```typescript
import { Component } from '@angular/core';
import { AuthService } from './auth.service';
import { Router } from '@angular/router';

@Component({
  selector: 'app-login',
  template: `
    <form (ngSubmit)="login()">
      <input [(ngModel)]="credentials.email" type="email" placeholder="Email" required>
      <input [(ngModel)]="credentials.password" type="password" placeholder="Password" required>
      <button type="submit">Login</button>
    </form>
  `,
})
export class LoginComponent {
  credentials = { email: '', password: '' };

  constructor(private authService: AuthService, private router: Router) {}

  login() {
    this.authService.login(this.credentials).subscribe(() => {
      this.router.navigate(['/dashboard']);
    });
  }
}
```

## 5. Adding Logout Functionality
```typescript
import { Component } from '@angular/core';
import { AuthService } from './auth.service';
import { Router } from '@angular/router';

@Component({
  selector: 'app-dashboard',
  template: `
    <h1>Dashboard</h1>
    <button (click)="logout()">Logout</button>
  `,
})
export class DashboardComponent {
  constructor(private authService: AuthService, private router: Router) {}

  logout() {
    this.authService.logout();
    this.router.navigate(['/login']);
  }
}
```

## Summary
- **AuthService**: Manages authentication state.
- **AuthGuard**: Protects routes.
- **AuthInterceptor**: Attaches JWT to requests.
- **LoginComponent**: Handles user login.
- **DashboardComponent**: Displays protected content.

## Resources
- [Angular Authentication Guide](https://angular.io/guide/security)
- [JWT Authentication](https://jwt.io)
