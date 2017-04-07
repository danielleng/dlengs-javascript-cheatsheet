# Daniel's Javascript Cheatsheet
My own Javascript cheatsheet for recalling Javascript's features and all its little nuances. Feel free to use it for your own reference.


#### Note
This is still a work in progress.


## Property Enumeration
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


## Execution contexts and Lexical Environment

More information available from https://medium.freecodecamp.com/lets-learn-javascript-closures-66feb44f6a44

1. Each function execution creates a new Execution Context.
2. Execution Contexts work like a stack, LIFO. (last in first out) We can see this in Chrome Dev Tool's callstack tab.
3. Each Execution Context has a few components, e.g. Lexical Environment and Variable Environment. (Or just "Environment")
4. Lexical Environment is a component used to define the association between identifiers and variables/functions based on the lexical nesting structure of ECMAScript code.
5. Additionally, the Lexical Environment has a reference to the outer Lexical Environment. (e.g. a function within a function, where inner function has reference to outer function's variables)
6. A new Lexical Environment is created each time a function is called, or code execution enters a block statement.

## Scope Chains

1. As per Lexical Environments discussed above, each Lexical Environment has access to its outer Environment. This set of identifiers that each environment has access to is called “scope.” We can nest scopes into a hierarchical chain of environments known as the “scope chain”.
2. This scope chain, or chain of environments associated with a function, is saved to the function object at the time of its creation. In other words, it’s defined statically by location within the source code. (This is also known as “lexical scoping”.)


## Closures

```javascript
let j = function() {
    let b = "hi";
    return function() { console.log(b); }
}

j()();  // hi
```

## Arrays

1. Converting arguments object to an array:
    `var args = Array.prototype.slice.call(arguments);`
    

## Currying Functions

1. Currying is constructing new functions from existing ones that allows partial application of the original function's arguments.
2. When creating a curried function, the curryer can accept some of the original function's arguments, and return a function that accepts the remaining arguments, and still return the same result when executed. 
3. Curryer:

    ```javascript
    const curryer = function(uncurried) {
        let args = Array.prototype.slice.call(arguments, 1);
        return function() {
            return uncurried.apply(this, args.concat(Array.prototype.slice.call(arguments, 0)));
        }
    }
    
    function otherFunction(arg1, arg2, arg3) {
        alert(arg1 + " " + arg2 + " " + arg3);
    }
    
    let curriedFunction = curryer(otherFunction, arg1, arg2);
    ```
    
4. 