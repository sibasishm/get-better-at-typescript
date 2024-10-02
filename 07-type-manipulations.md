Your types are more than just a way to catch errors at compile time. They help
reflect and communicate the business logic they represent.

## Generic Types

Generic types let you turn a type into a 'type function' which can receive
arguments. Like Pick and Omit, it takes in a type and a key, and return a new
type based on that key.

```ts
type InitialState<TData extends { id: number }, TError = unknown> = {
	data: T;
	error: Record<string, TError>;
	status: Record<string, 'loading' | 'success' | 'error'>;
};

type UserState = InitialState<{
	id: number;
	name: string;
	age: number;
}>;
```

The Multiverse of `extends`: In generic types, to set constraints on type
parameters In classes, to extend another class In interfaces, to extend another
interface

## Template Literal Types

Template literals are a way to create strings that can contain expressions, can
be used to interpolate other types into string types.

```ts
type ColorShade = 100 | 200 | 300 | 400 | 500 | 600 | 700 | 800 | 900;
type Color = 'red' | 'blue' | 'green';

type ColorPalette = `${Color}-${ColorShade}`;

const colorPalettes: ColorPalette[] = ['red-100', 'blue-200', 'green-300'];
```

## Conditional Types

Conditional types are a way to create types based on a condition.

```ts
type StatusType = { status: HttpResponseStatus };

type CreateEitherWithData<T> = {
	data: T extends StatusType ? T : StatusType & T;
	// if T can be assigned to StatusType, then data will be T
	// otherwise, it will be StatusType & T
};
```

## Mapped Types

Mapped types are a way to create types based on a mapping.

```ts
type StatusType = { status: HttpResponseStatus };

type CreateMappedType<T> = {
	[P in keyof T]: T[P] extends StatusType ? T[P] : StatusType & T[P];
};
```

## Type Guards

Type guards are a way to narrow the type of a variable based on a condition.

```ts
function isStatusType(value: unknown): value is StatusType {
	return typeof value === 'object' && value !== null && 'status' in value;
}
```

## Exercise

Let's create a type utility that prefixes all the keys of an object type with a
specific string.

```ts
type User = {
	name: string;
	age: number;
	email: string;
};

// The prefixed object type
type PrefixedUser = PrefixKeys<User, 'user_'>;
```

Let's implement a form validation system that checks if certain fields in a form
object are non-empty.

```ts
type UserForm = {
	name: string;
	age: number;
	email: string;
};

const form: UserForm = {
	name: 'John',
	age: 30,
	email: '',
};

const result = validateForm(form);
// Expected output:
// {
//   name: true;
//   age: true;
//   email: false;
// }
```
