# RxJS Operators

## What is RxJS?
RxJS (Reactive Extensions for JavaScript) is a library for reactive programming using Observables to handle asynchronous data streams.

## Types of RxJS Operators

### 1. **Creation Operators**
Used to create Observables.
- `of(value1, value2, ...)` – Emits values in sequence.
- `from(array | promise | iterable)` – Converts to an Observable.
- `interval(time)` – Emits values at intervals.
- `timer(delay, interval)` – Emits after a delay and optionally at intervals.

### 2. **Transformation Operators**
Used to transform data emitted by an Observable.
- `map(value => value * 2)` – Transforms each emitted value.
- `scan((acc, value) => acc + value, initialValue)` – Accumulates values over time.
- `switchMap(value => observable$)` – Cancels the previous Observable and switches to a new one.
- `mergeMap(value => observable$)` – Maps to an inner Observable and flattens results.

### 3. **Filtering Operators**
Used to filter emitted values.
- `filter(value => value > 10)` – Emits values that meet a condition.
- `take(n)` – Emits only the first `n` values.
- `skip(n)` – Skips the first `n` values.
- `distinctUntilChanged()` – Emits only distinct consecutive values.

### 4. **Combination Operators**
Used to combine multiple Observables.
- `concat(obs1, obs2)` – Emits values sequentially from multiple Observables.
- `merge(obs1, obs2)` – Merges multiple Observables concurrently.
- `combineLatest(obs1, obs2)` – Emits the latest values from multiple Observables.
- `forkJoin([obs1, obs2])` – Waits for all Observables to complete and emits the last values.

### 5. **Error Handling Operators**
Used to handle errors in Observables.
- `catchError(error => newObservable$)` – Catches errors and replaces them with a new Observable.
- `retry(n)` – Retries the Observable `n` times in case of an error.
- `retryWhen(errors => errors.pipe(delay(1000)))` – Retries based on logic.

### 6. **Utility Operators**
Used for debugging and managing Observables.
- `tap(value => console.log(value))` – Performs side effects without modifying values.
- `delay(time)` – Delays emission by a specified time.
- `timeout(time)` – Throws an error if no value is emitted within the specified time.
- `finalize(() => console.log("Completed"))` – Executes logic when the Observable completes.

## Common Use Cases
- Debouncing user input (`debounceTime(300)`) 
- API polling (`switchMap` with `interval`)
- Handling API failures (`catchError` and `retry`)
- Combining multiple HTTP calls (`forkJoin`, `combineLatest`)

## Example
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
- [RxJS Documentation](https://rxjs.dev)
- [Learn RxJS](https://www.learnrxjs.io)
