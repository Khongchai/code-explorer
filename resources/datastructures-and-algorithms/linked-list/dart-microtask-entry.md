https://github.com/dart-lang/sdk/blob/46039005697f04a88f3beefd08d729704d27216d/sdk/lib/async/schedule_microtask.dart#L63

When you schedule a task with 

```dart
scheduleMicrotask(() => print('Hello, world!'));
```

or 

```dart
Future.microtask(() => print('Hello, world!'));
```

Dart wraps the callback with [`_AsyncCallbackEntry`](https://github.com/dart-lang/sdk/blob/46039005697f04a88f3beefd08d729704d27216d/sdk/lib/async/schedule_microtask.dart#L9) class and adds it to the linked list.

```dart
/// Schedules a callback to be called as a microtask.
///
/// The microtask is called after all other currently scheduled
/// microtasks, but as part of the current system event.
void _scheduleAsyncCallback(_AsyncCallback callback) {
  _AsyncCallbackEntry newEntry = new _AsyncCallbackEntry(callback);
  _AsyncCallbackEntry? lastCallback = _lastCallback;
  if (lastCallback == null) {
    _nextCallback = _lastCallback = newEntry;
    if (!_isInCallbackLoop) {
      _AsyncRun._scheduleImmediate(_startMicrotaskLoop);
    }
  } else {
    lastCallback.next = newEntry;
    _lastCallback = newEntry;
  }
}
```

And then [upon message invocation from the vm](https://github.com/dart-lang/sdk/blob/46039005697f04a88f3beefd08d729704d27216d/sdk/lib/_internal/vm/lib/isolate_patch.dart#L176), Dart goes through the linked list and invokes all callbacks until the list is empty (this is why long microtask queues can freeze the UI).

```dart
// https://github.com/dart-lang/sdk/blob/46039005697f04a88f3beefd08d729704d27216d/sdk/lib/async/schedule_microtask.dart#L34
void _microtaskLoop() {
  for (var entry = _nextCallback; entry != null; entry = _nextCallback) {
    _lastPriorityCallback = null;
    var next = entry.next;
    _nextCallback = next;
    if (next == null) _lastCallback = null;
    (entry.callback)();
  }
}
```