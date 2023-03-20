# Javascript and Typescript

## Javascript Notes

### Basic

If you want to initialize with a certain value, these are good to know:

	Array.from('abcde')
	Array.from('x'.repeat(5))
	Array.from({length: 5}, (v, i) => i)   // gives [0, 1, 2, 3, 4]
	
To sum up the values in an array, you don't need a for loop. The reduce method takes up a single line:

    return numbersArray.reduce((previousValue, currentValue) => previousValue + currentValue, 0)
	
To concatenate strings with a separator (such as a space) you don't need a for loop either. You can use Array.join().

	const sentence = wordsArray => wordsArray.join(' ');
	
Ways to shorten code include the ternary expression and the arrow function expression (comparable to an expression body in C#). A combination of both is shown here:

	const even_or_odd = number => number % 2 === 0 ? 'Even' : 'Odd';

## Typescript Notes

### Basic

#### Optional Properties

https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#optional-properties

Marked with `?` after parameter name.

	function printName(obj: { first: string; last?: string }) {
	  // ...
	}
	// Both OK
	printName({ first: "Bob" });
	printName({ first: "Alice", last: "Alisson" }); 

In JavaScript, if you access a property that doesn’t exist, you’ll get the value undefined rather than a runtime error. Now, when you read from an optional property in TypeScript, you’ll have to check for undefined before using it.

The following gives you the error: "Object is possibly 'undefined'"

	function printName(obj: { first: string; last?: string }) {
		// Error - might crash if 'obj.last' wasn't provided!
		console.log(obj.last.toUpperCase());

Solved by providing a check.

	if (obj.last !== undefined) {
		// OK
		console.log(obj.last.toUpperCase());
	}

A safe alternative using modern JavaScript syntax:

	console.log(obj.last?.toUpperCase());

### Advanced

#### Destructuring Generic Type Argument

A way of *destructuring a generic type argument* (even though TS supports tuples directly:

	type TypeTuple<A, B> = [A, B];
	type Left<T> = T extends TypeTuple<infer A, infer B> ? A : never;
	type Right<T> = T extends TypeTuple<infer A, infer B> ? B : never;

Alternatively, if you have a certain structure in your type, you could do something like this:

	type Left<T extends TypeTuple<unknown, unknown>> = T[0];
	type Right<T extends TypeTuple<unknown, unknown>> = T[1];