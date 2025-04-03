# RxJS Operators

## What is RxJS?
RxJS (Reactive Extensions for JavaScript) is a library for reactive programming that makes it easy to work with asynchronous and event-driven data streams. It provides powerful operators to manipulate and combine Observables effectively.

## Types of RxJS Operators

### 1. **Creation Operators**
Creation operators allow you to create Observables from various sources.

- `of(...values)`: Creates an Observable that emits the given values sequentially.
  ```typescript
  import { of } from 'rxjs';
  of(1, 2, 3).subscribe(console.log); // Output: 1, 2, 3
  ```
- `from(array | promise | iterable)`: Converts an array, promise, or iterable into an Observable.
  ```typescript
  import { from } from 'rxjs';
  from([10, 20, 30]).subscribe(console.log); // Output: 10, 20, 30
  ```

### 2. **Transformation Operators**
Transformation operators modify the emitted values.

- `map(value => transformation)`: Applies a function to each emitted value.
  ```typescript
  import { of, map } from 'rxjs';
  of(1, 2, 3).pipe(map(x => x * 2)).subscribe(console.log); // Output: 2, 4, 6
  ```

- `scan((acc, value) => acc + value, initialValue)`: Accumulates values over time.
  ```typescript
  import { of, scan } from 'rxjs';
  of(1, 2, 3, 4).pipe(scan((acc, val) => acc + val, 0)).subscribe(console.log);
  // Output: 1, 3, 6, 10
  ```

### 3. **Filtering Operators**
Filtering operators allow selective emission of values.

- `filter(value => condition)`: Emits values that satisfy the given condition.
  ```typescript
  import { of, filter } from 'rxjs';
  of(1, 2, 3, 4, 5).pipe(filter(x => x % 2 === 0)).subscribe(console.log); // Output: 2, 4
  ```

### 4. **Combination Operators**
Combination operators allow multiple Observables to work together.

- `concat(obs1, obs2, ...)`: Concatenates multiple Observables sequentially.
  ```typescript
  import { concat, of } from 'rxjs';
  concat(of(1, 2, 3), of(4, 5, 6)).subscribe(console.log);
  // Output: 1, 2, 3, 4, 5, 6
  ```

- `merge(obs1, obs2, ...)`: Merges multiple Observables concurrently.
  ```typescript
  import { merge, interval } from 'rxjs';
  import { take } from 'rxjs/operators';

  merge(interval(1000).pipe(take(3)), interval(500).pipe(take(3)))
    .subscribe(console.log);
  // Output: Interleaved values from both Observables
  ```

### 5. **Error Handling Operators**
These operators help manage errors in an Observable stream.

- `catchError(error => newObservable)`: Catches and replaces an error with a new Observable.
  ```typescript
  import { throwError, of, catchError } from 'rxjs';
  throwError('Error!').pipe(catchError(err => of('Recovered'))).subscribe(console.log);
  // Output: Recovered
  ```

### 6. **Utility Operators**
These operators provide debugging and Observable lifecycle management tools.

- `tap(value => sideEffect)`: Performs a side effect without altering values.
  ```typescript
  import { of, tap } from 'rxjs';
  of(1, 2, 3).pipe(tap(x => console.log('Side effect:', x))).subscribe(console.log);
  // Output: Side effect: 1, 1, Side effect: 2, 2, Side effect: 3, 3
  ```

## Common Use Cases
- **Debouncing user input**: `debounceTime(300)` to prevent excessive API calls.
- **API polling**: Use `switchMap` with `interval` to fetch data periodically.
- **Handling API failures**: Use `catchError` and `retry` to manage errors.
- **Combining API calls**: Use `forkJoin` and `combineLatest` for efficient API responses.

## Example Usage
```typescript
import { of, filter, map } from 'rxjs';

of(1, 2, 3, 4, 5)
  .pipe(
    filter(value => value % 2 === 0),
    map(value => value * 10)
  )
  .subscribe(console.log); // Output: 20, 40
```

## Resources
- [RxJS Official Documentation](https://rxjs.dev)
- [Learn RxJS](https://www.learnrxjs.io)
