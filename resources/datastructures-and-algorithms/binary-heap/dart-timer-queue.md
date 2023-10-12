https://github.com/dart-lang/sdk/blob/3f3def4654660f9f0575e7e4d82e4ffcd90a432c/sdk/lib/_internal/vm/lib/timer_impl.dart#L7

When you do this

```dart
void main() {
    Timer.run(() => print('Hello, world!'));
}
```

Dart has to figure out when to run the scheduled timer. Dart compares all the timers in the array (array-based binary heap).

```dart
  // https://github.com/dart-lang/sdk/blob/3f3def4654660f9f0575e7e4d82e4ffcd90a432c/sdk/lib/_internal/vm/lib/timer_impl.dart#L73
  void _bubbleUp(_Timer timer) {
    while (!isFirst(timer)) {
      _Timer parent = _parent(timer);
      if (timer._compareTo(parent) < 0) {
        _swap(timer, parent);
      } else {
        break;
      }
    }
  }

  // https://github.com/dart-lang/sdk/blob/3f3def4654660f9f0575e7e4d82e4ffcd90a432c/sdk/lib/_internal/vm/lib/timer_impl.dart#L84
  void _bubbleDown(_Timer timer) {
    while (true) {
      var leftIndex = _leftChildIndex(timer._indexOrNext as int);
      var rightIndex = _rightChildIndex(timer._indexOrNext as int);
      _Timer newest = timer;
      if (leftIndex < _used && _list[leftIndex]._compareTo(newest) < 0) {
        newest = _list[leftIndex];
      }
      if (rightIndex < _used && _list[rightIndex]._compareTo(newest) < 0) {
        newest = _list[rightIndex];
      }
      if (identical(newest, timer)) {
        // We are where we should be, break.
        break;
      }
      _swap(newest, timer);
    }
  }

  // https://github.com/dart-lang/sdk/blob/3f3def4654660f9f0575e7e4d82e4ffcd90a432c/sdk/lib/_internal/vm/lib/timer_impl.dart#L84
  void _swap(_Timer first, _Timer second) {
    var newFirstIndex = second._indexOrNext as int;
    var newSecondIndex = first._indexOrNext as int;
    first._indexOrNext = newFirstIndex;
    second._indexOrNext = newSecondIndex;
    _list[newFirstIndex] = first;
    _list[newSecondIndex] = second;
  }
```