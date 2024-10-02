## Intersection Types

An intersection type lets us combine multiple object types into a single type.
It uses the `&` operator.

```ts
type User1 = {
	age: number;
};

type User2 = {
	age: string;
};

type User = User1 & User2;
```

What type do you think `User.age` is?

## Types or Interfaces

You should use types by default until you need a specific feature of interfaces,
like 'extends'.

But it's a common mistake to think of them as interchangeable. They're not.

1. interface extends a. Better Errors When Merging Incompatible Types b. the
   intersection is recomputed every time it's used which can be slow
2. A type can represent anything – union types, object types, intersection
   types, and more while interface can only represent object types.
3. TS automatically merges multiple interfaces with the same name in the same
   scope are created

## Dynamic Object Keys

TypeScript prefers to be conservative. It's not going to let you add keys to an
object that it doesn't know about.

```ts
const user: {
  [index: string]: string | number;
} = {};

cosnt user: Record<'name' | 'age' | 'email', string> & {
  [index: string]: number;
} = {};

// Record can also support a literal type as keys, but an index signature can't:

const user = {
	name: 'John',
  address: '123 Main St',
	email: 'john@example.com',
};

user.age = 30;
```

`object` is a type that represents represents any non-primitive type. This
includes arrays, functions, and objects. object type is rarely useful as the
error messages are not very helpful, prefer using Record<string, unknown>
instead.

`{}` will accept any value except `null` and `undefined`.

```ts
interface User {
	id: number;
	name: string;
}

function printUser(user: User) {
	Object.keys(user).forEach(key => {
		// Object.keys returns type string[] and not [keyof User]
		console.log(user[key]);
	});
}
```

When it comes to iterating over object keys, there are two main choices for
handling this issue: you can either make the key access slightly unsafe via as
keyof typeof, or you can make the type that's being indexed into looser.

```ts
function printUser(user: User) {
	for (const key in user) {
		console.log(user[key as keyof typeof user]);
	}
}

function printUser(obj: Record<string, unknown>) {
	Object.keys(obj).forEach(key => {
		console.log(obj[key]);
	});
}

function printUser(user: User) {
	Object.values(user).forEach(console.log);
}
```

## Utility Types

A collection of essential types that TypeScript doesn't provide by default.
https://github.com/sindresorhus/type-fest

1. The `Partial` utility type lets you create a new object type from an existing
   one, except all of its properties are optional.
2. Opposite of Partial is the `Required` utility type. However, both only work
   one level deep
3. The `Pick` utility type lets you create a new object type from an existing
   one, but only include the properties that you specify.
4. The `Omit` utility type is opposite of Pick but when using Omit, you are
   [able to exclude](https://github.com/microsoft/TypeScript/issues/30825#issuecomment-523668235)
   properties that don't exist on an object type.
