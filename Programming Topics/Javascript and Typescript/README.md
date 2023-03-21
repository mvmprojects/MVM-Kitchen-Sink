# Javascript and Typescript

## Javascript Notes

### Basic

#### Organizing Code

Ways to shorten code include the ternary expression and the arrow function expression (comparable to an expression body in C#). A combination of both is shown here:

	const even_or_odd = number => number % 2 === 0 ? 'Even' : 'Odd';

#### Arrays

If you want to initialize with a certain value, these are good to know:

	Array.from('abcde')
	Array.from('x'.repeat(5))
	Array.from({length: 5}, (v, i) => i)   // gives [0, 1, 2, 3, 4]
	
Where `{ length: 5}` is an object that will be converted to an array with a length of 5, but each position will be undefined and so `v` will also be undefined. The current index will be used as the value to place onto the position, hence the values of 0, 1, 2, 3 and 4.

To sum up the values in an array, you don't need a for loop. The reduce method takes up a single line:

    return numbersArray.reduce((previousValue, currentValue) => previousValue + currentValue, 0)
	
To concatenate strings with a separator (such as a space) you don't need a for loop either. You can use Array.join().

	const sentence = wordsArray => wordsArray.join(' ');
	
To create a range of numbers, of a length n with a starting value of x, you can do the following:

	const countBy = (x, n) => Array.from({length: n}, (v, k) => (k + 1) * x)
	console.log(countBy(1,10)); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
	
### Advanced
	
Although you can use Array.filter() to get only the items you want, you might want to do a comparison between two arrays and only pick up the items that appear in both. There is no Array.intersect() method in javascript (at the time of writing) unless you add a library.

To create your own intersect method, you can traverse the longer array and then keep checking if the item at the current index is present in the shorter array. 

	function intersect(a, b) {
		var temp;
		if (b.length > a.length) temp = b, b = a, a = temp;
		return a.filter(function (e) {
			return b.indexOf(e) > -1;
		});
	}

Please keep in mind the problem of quadratic growth of operations (n \* n) when the arrays are very long.

Note that the above method will not skip duplicates. To remove duplicates, an extra step is needed.

	function intersect(a, b) {
		var temp;
		if (b.length > a.length) temp = b, b = a, a = temp;
		return a
			.filter(function (element) {
				return b.indexOf(element) > -1;
			})
			.filter(function (element, index, calledArray) { // extra step to remove duplicates
				return calledArray.indexOf(element) === index;
			});
	}

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