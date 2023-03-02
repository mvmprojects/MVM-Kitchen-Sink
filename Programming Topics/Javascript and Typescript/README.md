# Javascript and Typescript

## Javascript

If you want to initialize with a certain value, these are good to know:

	Array.from('abcde'), Array.from('x'.repeat(5))
	
or 

	Array.from({length: 5}, (v, i) => i)   // gives [0, 1, 2, 3, 4]

## Typescript

A way of *destructuring a generic type argument* (even though TS supports tuples directly:

	type TypeTuple<A, B> = [A, B];
	type Left<T> = T extends TypeTuple<infer A, infer B> ? A : never;
	type Right<T> = T extends TypeTuple<infer A, infer B> ? B : never;

Alternatively, if you have a certain structure in your type, you could do something like this:

	type Left<T extends TypeTuple<unknown, unknown>> = T[0];
	type Right<T extends TypeTuple<unknown, unknown>> = T[1];