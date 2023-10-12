# Contributing to Code Explorer 🛠️

Thanks for showing an interest in contributing! Here are a few guide lines to adhere to to make sure that as the project grows, people can still navigate it easily.

## Folder Structure

```
📦 Code-Explorer (root)
 ┣ 📂 resources
 ┃ ┣ 📂 data-structures-and-algorithms
 ┃ ┃ ┣ 📂 binary-heap
 ┃ ┃ ┃ ┣ 📜 dart-timer-queue.md
 ┃ ┃ ┃ ┗ 📜 java-scheduler-queue.md
 ┃ ┃ ┗ ... (more algorithms)
 ┃ ┗ 📂 design-patterns
 ┃   ┣ 📂 flyweight
 ┃   ┃ ┣ 📜 project-1.md
 ┃   ┃ ┗ 📜 project-2.md
 ┃   ┗ ... (more design patterns)
 ┣ 📜 CONTRIBUTING.md
 ┗ 📜 README.md
```

- All new content must be under resources
- All content must be in English. You are encouraged to make a similar project, in your language.
- New categories are welcome, but make sure they are put in the same level as algorithms.

## Including Images

Not sure about this yet. Let's see.

## Naming

- All file names must be kebab-case.
- Must be in English.
- The naming can be whatever, just make sure it describes **where it's from** and if possible **what it is used for**, for example, a heap can be used to implement both search and sort algo. Make sure you specify.

## File Content

### Data Strcutures and Algorithms

If possible, the content of the file must begin with a link to the source code. For example, in `resources/algorithms/binary-heap/dart-timer-queue.md`, it might look like this

----------------content begin---------------------

https://github.com/dart-lang/sdk/blob/3f3def4654660f9f0575e7e4d82e4ffcd90a432c/sdk/lib/_internal/vm/lib/timer_impl.dart#L7

When you use a Timer class in Dart, the inner queue structure is implemented using a flavor of the binary heap algorithm:

```dart
class _TimerHeap {
  List<_Timer> _list;
  int _used = 0;
//... don't have to show the whole thing.
}
```

----------------content end---------------------

The link should be tied to a particular commit. The content of the file can be anything: there can be links to a blog post that explains what the source code does, or you can write out in your own words.

### Design Patterns

This one is a bit harder to pinpoint where exactly a pattern is used in a source code, I might be a bit more lenient here. The link at the top of the file might link to the search result of a repo or the root of a folder.
For example, in `resources/design-patterns/command/novu-command-mapper.md`.

----------------content begin---------------------
https://github.com/search?q=repo%3Anovuhq%2Fnovu%20command&type=code

Novu is an open-source notification infrastructure...more explanation

They use the command pattern to pass the data between controllers and the lower layers.

_more stuff + some examples_
----------------content end---------------------
