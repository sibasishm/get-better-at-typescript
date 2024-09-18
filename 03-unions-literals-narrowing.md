When a value is one of many possible types, you can use a union type to narrow
the type of a variable.

## Union Types

To create a union type, the | operator is used to separate the types. Each type
of the union is called a 'member' of the union. A union type is TypeScript's way
of saying that a value can be "either this type or that type".

## Literal Types

Just as TypeScript allows us to create union types from multiple types, it also
allows us to create types which represent a specific primitive value. These are
called literal types.

```ts
type StatusCodeLiteral = 200 | 404 | 500;

type StatusCodeUnion = number | null;
```

Literal types are useful when you want to create a type which represents a
specific value, but you don't want to use a union type.

## Wider vs Narrower Types

```ts
const recordOfSizes = {
	small: 'small',
	large: 'large',
};

const logSize = (size: 'small' | 'large') => {
	console.log(recordOfSizes[size]);
};

logSize('medium');
```

## Type Narrowing or Type Guards

1. using `typeof` operator for primitive types or `instanceof` and `in` operator
   for non-primitive types
2. conditional operators like && and ||, will take the truthiness into account
   for coercing the boolean value
3. throw errors or return early to narrow types
4. use `as` operator to cast types

```ts
function validateUsername(username?: string): boolean {
	return username.length > 5;

	return false;
}

async unction fetchUser(username?: string): Promise<User> {
	// validateUsername will narrow the type of username to string

	const data = await fetch(`/api/users/${username}`);
}

type APIResponse = | {
      data: {
        id: string
      }
    }
  | {
      data?: undefined
      error: string
    }

const handleResponse = (response: APIResponse) => {
  if ('data' in response) {
    return response.data.id
  } else {
    throw new Error(response.error)
  }
}
```

## unknown and never

TypeScript's widest type is unknown. It represents something that we don't know
what it is. All other types can be assigned to unknown.

any can be assigned to anything, and anything can be assigned to any. any is
both narrower and wider than every other type.

unknown, on the other hand, is part of TypeScript's type system. It's wider than
every other type, so it can't be assigned to anything.

```ts
function logUpperCase(value: unknown) {
	console.log(input.toUppercase());
}
```

The never type represents something that can never happen. You cannot assign
anything to never, except for never itself. However, you can assign never to
anything:

```ts
const somethingDangerous = () => {
	if (Math.random() > 0.5) {
		throw new Error('Something went wrong');
	}

	return 'all good';
};

try {
	somethingDangerous();
} catch (error) {
	if (true) {
		console.error(error.message);
		//'error' is of type 'unknown'.
	}
}

const parseValue = (value: unknown) => {
	if (typeof value === 'object') {
		return value.data.id;
	}
	throw new Error('Invalid value');
};
```

Use a library like Zod to parse and validate data on the run time.

## Discriminated Unions

A discriminated union is a type that can have one of many types, but each type
can have different properties.

```ts
interface Cat {
	name: string;
	age: number;
	color: string;
}

interface Dog {
	name: string;
	age: number;
	breed: string;
}

type Animal = Cat | Dog;
```

I have written
[a blog post](https://www.sibas.in/blogs/type-guards-and-advanced-union-types)
about it to use type guards to narrow the discriminated union type.

```ts
type Only<T, U> = { [P in keyof T]: T[P] } & Omit<
	{ [P in keyof U]?: never },
	keyof T
>;

type CatOnly = Only<Cat, Dog>; // Results in { name: string; age: number; color: string; }
type DogOnly = Only<Dog, Cat>; // Results in { name: string; age: number; breed: string; }

type Either<T, U> = Only<T, U> | Only<U, T>;

type Animal = Either<Cat, Dog>;
```
