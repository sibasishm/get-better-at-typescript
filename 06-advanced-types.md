Let's look at some techniques of deriving types from other types. This lets us
reduce repetition in our code, and create a single source of truth for our
types.

Based on different types, sharing a common concern either derive them or create
new types and decouple them.

## keyof and typeof

The keyof operator allows you to extract the keys from an object type into a
union type. The typeof operator allows you to extract a type from a value but
there is no way to to create a value from a type.

typeof is not the same as the typeof operator used at runtime.

## Indexed Access Types

Indexed access types are a way to create a type from a value. The value must be
an object, and the type is the type of the property with the given key.

```ts
type User = {
	name: string;
	age: number;
	guardian: {
		name: string;
	};
};

type UserName = User['name'];

type GuardianName = User['guardian']['name'];
```

## as const

```ts
const userRoles = {
	ADMIN: 'admin',
	MODERATOR: 'moderator',
	USER: 'user',
} as const;

type UserRolesType = (typeof userRoles)[keyof typeof userRoles];

function handleRoleChange(userRole: UserRolesType) {
	// ...
}
```

1. Enum requires you to pass the enum value while as const allows you to pass
   raw string
2. Enums have to be imported
3. Enums types are based on the name of the enum, meaning that enums with the
   same values that come from different enums are not compatible.

## Function Types

Deriving types from a function is more useful for functions that you don't
control like 3rd party libraries that might not export the accompanying types.

1. The `Parameters` utility type lets you extract the types of the parameters of
   a function.
2. The `ReturnType` utility type lets you extract the return type of a function.
3. The `Awaited` utility type lets you extract the return type of a function
   that returns a promise.

## Transforming Types

1. The `Exclude` utility type is used to remove types from a union.
2. `NonNullable` is used to remove null and undefined from a type.
3. `Extract` is the opposite of Exclude. It's used to extract types from a
   union.

```ts
type T0 = Omit<{ a: string; b: string }, 'a'>; //  { b: string; }, a is removed, works on Objects to exclude properties
type T1 = Exclude<{ a: string; b: string }, 'a'>; // { a: string, b: string }, a does not extend { a: string, b: string } so Exclude does nothing

type T2 = Omit<string | number, string>; // Attempts to remove all string keys (basically all keys) from string | number , we get {}
type T3 = Exclude<string | number, string>; // string extends string so is removed from the union so we get number
```
