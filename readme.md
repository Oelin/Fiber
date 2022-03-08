# Fiber

Fiber is a 55 byte one-liner which adds reactive programming to JavaScript. Here's the entire library:

```js
export default (value, ...use) => to =>
	to.call
		? use.push(to) + to(value)
		: use.forEach(use => use(value=to))
```

In compressed form:

```js
export default (v,...u)=>t=>t.call?u.push(t)+t(v):u.forEach(u=>u(v=t))
```

## Usage

What the library exports is a special kind of variable that can be monitored for changes. Here's a simple example:

```js
import fiber from './fiber.js'

const name = fiber('Alice')

// run this once and whenever `name` changes

name(console.log)

// console outputs "Alice"

name('Sam')

// console outputs "Sam"
```
