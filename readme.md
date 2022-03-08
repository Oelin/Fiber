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

## Example: Reactive Element Properties

Add this function to your project:

```js
function bind(fibers, element) {

	Object.entries(fibers).forEach(([key, fiber]) => {
		fiber(value => element[key] = value)
	})
}
```

Now you can make any element's properties reactive.

```js
const div = document.createElement('div')
const id = fiber('neutral')

bind(div, { id })
```

```js
console.log(div)

// <div id='neutral'></div>
```

```js
id('danger')

console.log(div)

// <div id='danger'></div>
```

## Example: Reactive Components

Taking this idea further, we can simplify the construction of components

```js
function App({ background, ...rest }) {

	const root = document.createElement('div')
	const image = document.createElement('img')
	
	root.append(image)
	
	bind(root, rest)
	bind(image, { src: background })
	
	return root
}
```

```js
const background = fiber('./cat.jpg')

const app = App({
	background,
	id: fiber('my-app')
})

console.log(app)

// <div id='my-app'>
//     <img src='./cat.jpg'>
// </div>

background('./dog.jpg')

console.log(app)

// <div id='my-app'>
//     <img src='./dog.jpg'>
// </div>
```
