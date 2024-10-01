## let vs const

```ts
type UserRoles = 'admin' | 'moderator' | 'user';

let user1 = 'admin';

const handleRoleChange = (userRoles: UserRoles) => {
	// ...
};

handleRoleChange(user1);
// Argument of type 'string' is not assignable to parameter of type 'UserRoles'.
```

As a solution you can specify the type or declare the variable as const. But
objects are mutable by default even though they are declared as const.

```ts
type UserRoles = {
	role: 'admin' | 'moderator' | 'user';
};

const user1 = {
	role: 'admin',
};

const handleRoleChange = (userRoles: UserRoles) => {
	// ...
};

handleRoleChange(user1);
```

The solution is to use inline object or adding a type to the variable. When
inlining the object, TypeScript knows that there is no way that status could be
changed before it is passed into the function, so it infers it more narrowly.

## const vs readonly

```ts
type User = {
	readonly name: string;
	readonly age: number;
	role?: 'admin' | 'moderator' | 'user';
};
```

Note that this only occurs on the type level. At runtime, the properties are
still mutable. The `Readonly` utility type can be used to create a new type that
has all properties of the original type marked as readonly.

## Immutability

Immutability is a core concept in functional programming.

```ts
const user: User = {
	name: 'John',
	age: 30,
	role: 'admin',
};
```

Hovering over the `user.role` shows
`(property) role?: "admin" | "moderator" | "user" | undefined`. The fix is to
get TypeScript to infer it properly would be to somehow mark the entire object,
and all its properties, as immutable. This would tell TypeScript that the object
and its properties can't be changed, so it would be free to infer the literal
types of the properties.

The `as const` assertion has made the entire object deeply read-only, including
all of its properties. This means that `user.role` is now inferred as the
literal type "admin".

The as const assertion is overridden by the variable annotation.

As compared to `Object.freeze` which gives run-time immutability, the as const
assertion is more strict as it deeply makes the object immutable.

## Enums are strange

There is no equivalent syntax in JavaScript to the enums. Hence the
transpilation and behavior are rather
[weird.](https://youtu.be/0fTdCSH_QEU?si=tzBhfOHzuAt5wfV5)
