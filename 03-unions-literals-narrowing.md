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

## Type Narrowing

1. using `typeof` operator for primitive types or `instanceof` and `in` operator
   for non-primitive types
2. conditional operators like && and ||, will take the truthiness into account
   for coercing the boolean value
3. throw errors or return early to narrow types

```ts
function validateUsername(username: string | null): boolean {
	return username.length > 5;

	return false;
}
```
