# NgRx State Management

## What is NgRx?
NgRx is a state management library for Angular applications, based on Redux. It provides reactive state management using RxJS, making it easy to manage and share state across components.

## Core Concepts
NgRx is built on the following key concepts:

- **Store**: A single immutable state container.
- **Actions**: Events that describe state changes.
- **Reducers**: Pure functions that handle state transitions.
- **Selectors**: Functions to extract and derive state data.
- **Effects**: Side effects handling (e.g., API calls).

## Installation
```sh
npm install @ngrx/store @ngrx/effects @ngrx/entity @ngrx/store-devtools
```

## Setting Up NgRx

### 1. Define Actions
```typescript
import { createAction, props } from '@ngrx/store';

export const loadStudents = createAction('[Student] Load Students');
export const loadStudentsSuccess = createAction(
  '[Student] Load Students Success',
  props<{ students: any[] }>()
);
export const loadStudentsFailure = createAction(
  '[Student] Load Students Failure',
  props<{ error: string }>()
);
```

### 2. Create a Reducer
```typescript
import { createReducer, on } from '@ngrx/store';
import { loadStudents, loadStudentsSuccess, loadStudentsFailure } from './student.actions';

export interface StudentState {
  students: any[];
  loading: boolean;
  error: string | null;
}

const initialState: StudentState = {
  students: [],
  loading: false,
  error: null
};

export const studentReducer = createReducer(
  initialState,
  on(loadStudents, state => ({ ...state, loading: true })),
  on(loadStudentsSuccess, (state, { students }) => ({ ...state, loading: false, students })),
  on(loadStudentsFailure, (state, { error }) => ({ ...state, loading: false, error }))
);
```

### 3. Register the Store in the Module
```typescript
import { NgModule } from '@angular/core';
import { StoreModule } from '@ngrx/store';
import { studentReducer } from './state/student.reducer';

@NgModule({
  imports: [StoreModule.forRoot({ students: studentReducer })],
})
export class AppModule {}
```

### 4. Use Selectors to Get Data
```typescript
import { createSelector, createFeatureSelector } from '@ngrx/store';
import { StudentState } from './student.reducer';

export const selectStudentState = createFeatureSelector<StudentState>('students');
export const selectAllStudents = createSelector(selectStudentState, state => state.students);
export const selectLoading = createSelector(selectStudentState, state => state.loading);
```

### 5. Dispatch Actions and Select State in Components
```typescript
import { Component } from '@angular/core';
import { Store } from '@ngrx/store';
import { loadStudents } from './state/student.actions';
import { selectAllStudents } from './state/student.selectors';

@Component({
  selector: 'app-student',
  template: `
    <div *ngIf="loading$ | async">Loading...</div>
    <ul>
      <li *ngFor="let student of students$ | async">{{ student.name }}</li>
    </ul>
    <button (click)="loadData()">Load Students</button>
  `
})
export class StudentComponent {
  students$ = this.store.select(selectAllStudents);
  loading$ = this.store.select(selectLoading);

  constructor(private store: Store) {}

  loadData() {
    this.store.dispatch(loadStudents());
  }
}
```

### 6. Handling Side Effects with Effects
```typescript
import { Injectable } from '@angular/core';
import { Actions, createEffect, ofType } from '@ngrx/effects';
import { loadStudents, loadStudentsSuccess } from './state/student.actions';
import { mergeMap, map } from 'rxjs/operators';
import { StudentService } from '../services/student.service';

@Injectable()
export class StudentEffects {
  loadStudents$ = createEffect(() =>
    this.actions$.pipe(
      ofType(loadStudents),
      mergeMap(() => this.studentService.getStudents()
        .pipe(
          map(students => loadStudentsSuccess({ students }))
        )
      )
    )
  );

  constructor(private actions$: Actions, private studentService: StudentService) {}
}
```

## Debugging with NgRx DevTools
```typescript
import { StoreDevtoolsModule } from '@ngrx/store-devtools';

@NgModule({
  imports: [StoreDevtoolsModule.instrument({ maxAge: 25 })],
})
export class AppModule {}
```

## Summary
NgRx provides a powerful state management solution for Angular applications. By using actions, reducers, effects, and selectors, we can manage complex state efficiently and in a scalable manner.

## Resources
- [NgRx Official Documentation](https://ngrx.io)
- [NgRx GitHub](https://github.com/ngrx/platform)
