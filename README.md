# Daniel's Javascript Cheatsheet
My own Javascript cheatsheet for recalling Javascript's features and all its little nuances. Feel free to use it for your own reference.


### Note
This is still a work in progress.


## Property enumeration
Note that *for...in* loop may iterate through prototype properties/methods, hence check with `hasOwnProperty`.

```javascript
let jack = {
	type: "human",
	age: 15,
	skill: "karate",
	height: "190cm"
}
for ( let key in jack ) {
	if (jack.hasOwnProperty(key)) {
		// do stuff with jack[key]
	}
}
```

```javascript
// Create non-enumerable property using defineProperty
Object.defineProperty(jack, "level", { enumerable: false, value: 25 });
```

Using Javascript propertiesObject argument to initialize object properties:

```javascript
{ 
  foo: {
    enumerable: false,
    writable: true,
    configurable: true,
    value: 'hello'
  },
  // bar is a getter-and-setter (accessor) property
  bar: {
    configurable: false,
    get: function() { return 10; },
    set: function(value) {
      console.log('Setting `o.bar` to', value);
  }
}
```

## Function Declarations and Function Expressions
1. Declaration: 

   ```javascript
   function abc () {}
   ```

2. Expression:

   ```javascript
   let abc = function() {}
   ```

3. Function declarations are hoisted.

   ```javascript
   function foo(){
       function bar() {
           return 3;
       }
       return bar();
       function bar() {
           return 8;
       }
   }
   alert(foo()); // 8
   ```

4. Function expressions are also hoisted, but follow the rules of **let, const** and **var**.

   ```javascript
   function foo(){
       var bar = function() {
           return 3;
       };
       return bar();
       var bar = function() {	// This assigment will never run
           return 8;
       };
   }
   alert(foo()); // 3
   ```
   
   In this case, declaration for `bar` is hoisted up **twice**. 
   However, once code execution starts, the return statement prevents the next assignment from being executed.



## let & const

1. **Block**-level scope.
2. Hoisted, but will produce ReferenceError if accessed before assigned.
3. `let` and `const` - cannot be re-declared if in the same block.
4. `const` - value cannot be changed or reassigned, but is NOT IMMUTABLE.


## Class Syntax

Class declarations are not hoisted.

```javascript
class Rectangle {
	constructor(width, height) {
		this.width = width;
		this.height = height;
	}

	get area() {
    	return this.calcArea();
  	}

	calcArea() {
		return this.height * this.width;
	}
}


const square = new Rectangle(10, 10);
console.log(square.area);
```


## Prototypal Inheritance

```javascript
let Species = function(legs) {
	this.legs = legs; 	
	this.height = 0;	
}
Species.prototype.grow = function() { this.height++; }

let Human = function() { }
Human.prototype = new Species(2);
Human.prototype.getHeight = function() { return this.height; }

let dave = Object.create(Human.prototype);
dave.name = "Alex";
```


## Execution contexts, the stack and Lexical Environment

1. Each function execution creates a new Execution Context.
2. Execution Contexts work like a stack, LIFO. (last in first out)
3. Each Execution Context has a few features, e.g. Lexical Environment and Variable Environment. (Or just "Environment")

