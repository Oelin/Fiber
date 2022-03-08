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

The library exports is a special kind of variable that can be monitored for value changes. For example:

1. Import the library 

```js
import fiber from './fiber.js'
```

2. Create a new fiber variable. Its initial value is set to `'Alice'`.

```js
const name = fiber('Alice')
```

3. Register an event listener. In this case, we've registered `console.log()`, so any changes to `name` will be output to the console. Note that the listener is run once when first registered.

```js
name(console.log)

// console outputs 'Alice'

name('Sam')

// console outputs 'Sam'
```

## Adding Multiple Listeners

You can register any number of listeners to a fiber variable. For example:

```js

function showAge(age) {
	console.log(`Age is currently ${age}`)
}

function showYoung(age) {
	if (age < 20) {
		console.log('You are young!')
	}
}

const myAge = fiber(19, showAge, showYoung)

// console outputs 'Age is currently 19' and then 'You are young'

myAge(30)

// console outputs 'Age is currently 30'
```
