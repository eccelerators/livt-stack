# Stack Component for Livt

This package provides a fixed-size LIFO stack in Livt for small byte-oriented
storage needs.

This repository is intentionally small, so the core design rationale is captured
directly in this README rather than a separate design note.

## 📋 Overview

The current package is organized around one main component:

- `Stack` stores up to 32 entries of `logic[8]` and returns values in last-in,
  first-out order.

The stack is currently fixed to:

- 32 elements of storage
- 8-bit `logic[8]` stack entries

The public API is intentionally small and centered on predictable LIFO behavior:

- `Push()` appends to the top of the stack when space is available.
- `Pop()` removes and returns the newest stacked element.
- `Peek()` reads the newest stacked element without removing it.
- `IsEmpty()`, `IsFull()`, and `GetPosition()` expose stack state.
- `Clear()` resets the stack to an empty state.

## 📁 Project Structure

```text
.
├── src/
│   └── Stack.lvt
├── tests/
│   └── StackTest.lvt
├── LICENSE
├── README.md
└── livt.toml
```

## 🔨 Building

Build the package with:

```bash
livt build
```

The package configuration is defined in [`livt.toml`](livt.toml). The current
project name there is `Stack`.

## 🧪 Running Tests

Run the full test suite with:

```bash
livt test
```

Configured test components:

- `StackTest`

## 📚 Component Guide

### `Stack`

Small fixed-size LIFO stack in the `Livt.Collections` namespace.

Features:

- array-backed storage
- LIFO pop order
- explicit empty and full state checks
- defined empty-stack return value for `Peek()` and `Pop()`

Public methods:

- `Push(value: logic[8])`
- `Pop() logic[8]`
- `Peek() logic[8]`
- `IsEmpty() bool`
- `IsFull() bool`
- `GetPosition() int`
- `Clear()`

`Push()` is ignored when the stack is full. `Pop()` and `Peek()` return
`0b0000_0000` when the stack is empty.

## 💡 Example

```livt
using Livt.Collections

component Example
{
    stack: Stack

    new()
    {
        this.stack = new Stack()
    }

    public fn PushAndPop(value: logic[8]) logic[8]
    {
        this.stack.Push(value)
        return this.stack.Pop()
    }
}
```

## 🔧 Configuration

This package does not currently expose configurable width or depth parameters.

To change the stack shape today, update the fixed constants and storage type in:

- [`src/Stack.lvt`](src/Stack.lvt)

If the stack contract changes, the expected behavior should also be updated in:

- [`tests/StackTest.lvt`](tests/StackTest.lvt)

## 📝 Notes

- The package is intentionally minimal and focused on predictable LIFO behavior.
- The stack implementation uses an array and a position counter.
- The empty-stack fallback value is currently fixed to `0b0000_0000`.

## 🤝 Contributing

Contributions are welcome. Areas that would be natural extensions for this
package include:

- configurable stack depth
- configurable entry width
- explicit overflow reporting for rejected `Push()` calls
- alternative empty-read behavior beyond returning `0b0000_0000`
- convenience wrappers for common storage patterns

## 📄 License

This project is licensed under the MIT License. See [LICENSE](LICENSE).
