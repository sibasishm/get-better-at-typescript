Annotations will often use a : - this is used to tell TypeScript that a variable
or function parameter is of a certain type.

```ts
let example1: string = 'Hello World!';
let example2: number = 42;
let example3: boolean = true;
let example4: symbol = Symbol();
let example5: bigint = 123n;
let example6: null = null;
let example7: undefined = undefined;
```

## Type Inference

TypeScript can infer a lot from the context that your code is run. You can
mostly not annotate variables but function parameters always need annotations,
so does Object literal types. When you don't annotate it, it defaults the type
to any - a scary, unsafe type.

## The `any` type

Using any can be used to turn off errors in TypeScript. It can be a useful
escape hatch for when a type is too complex to describe. But over-using any
defeats the purpose of using TypeScript, so it's best to avoid using it whenever
possible– whether implicitly or explicitly.

## Type alias

Using a type alias is a great way to ensure there's a single source of truth for
a type definition v/s inline types, which makes it easier to make changes in the
future. This is useful when sharing types in multiple places, or when a type
definition gets too large.

## Array<T> vs T[]

Using keyof with T[] can lead to unexpected results, The fix is to use Array<T>
instead. read
[this article](https://www.totaltypescript.com/array-types-in-typescript) for
more information.

## Tuples

Tuples let you specify an array with a fixed number of elements, where each
element has its own type.

```ts
// Tuple
let album: [string, number] = ['Rubber Soul', 1965];

// Array
let albums: string[] = [
	'Rubber Soul',
	'Revolver',
	"Sgt. Pepper's Lonely Hearts Club Band",
];
```

## Passing types to functions

```ts
const state = React.useState();

const typedState = React.useState<string>();

const audioElement = document.getElementById<HTMLAudioElement>("player");
// hovering over .getElementById shows:
(method) Document.getElementById(elementId: string): HTMLElement | null

const audioElement = document.querySelector<HTMLAudioElement>("#player");
```

Most functions in TypeScript can't receive types. So, to tell whether a function
can receive a type argument, hover it and check whether it has any angle
brackets.

## Typing functions

```ts
const logAlbumInfo = (
	title: string,
	// Default Parameters
	trackCount: number = 10, // The annotation of : string can also be omitted
	isReleased: boolean,
	// Optional Parameters
	releaseDate?: string,
	// Rest Parameters
	...otherTracks: string[]
): string => {
	// Functional return type
	// ...
};
```

Function types

```ts
// Optional parameters
type WithOptional = (index?: number) => number;

// Rest parameters
type WithRest = (...rest: string[]) => number;

// Multiple parameters
type WithMultiple = (first: string, second: string) => number;

// Some functions don't return anything. They perform some kind of action,
// but they don't produce a value.
type VoidFunction = () => void;
```

Async fucntion must return a Promise that resolves to a value of the same type
as the return type of the function.

```ts
const getUser = async (id: string): Promise<User> => {
	const user = await db.users.get(id);

	return user;
};
```
