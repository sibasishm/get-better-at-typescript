Type annotations and assertions can be an overkill at times.

```ts
type Nominee = {
	name: string;
	age: number;
	guardian:
		| {
				name: string;
				age: number;
		  }
		| string;
};

const user1: Nominee = {
	name: 'John',
	age: 30,
	guardian: {
		name: 'Mike',
		age: 44,
	},
};

const user2: Nominee = {
	name: 'John',
	age: 30,
	guardian: 'Steve',
};

console.log(user1.guardian.name);
console.log(user2.guardian);
```

When you annotate a variable, you're telling TypeScript that it's of a certain
type. While if you don't annotate a variable, TypeScript will infer the type
based on the value.

So we find a middle way out with `satisfies` and `asserts`. The satisfies
operator is a way to tell TypeScript that a value must satisfy certain criteria,
but still allow TypeScript to infer the type.

## Type Assertion

It's a way to override TypeScript's type inference and tell it to treat a value
as a different type.

```ts
location.state as {
	id: string;
	name: string;
};

transactionAmount as unknown as number;

const searchParams = new URLSearchParams(window.location.search);

const id = searchParams.get('id')!;
```

This `as` is a little unsafe! If transactionAmount is somehow not actually
passed in the naviagte, it will be null at runtime but number at compile time.

So, as does have some built-in safeguards. But by using as unknown as X, you can
easily bypass them. And because as does nothing at runtime, it's a convenient
way to lie to TypeScript about the type of a value.

## Error Supression Directives

1. Disable linter rules for a specific line of code
2. `@ts-expect-error` directive gives us a way to expect an error to occur on
   the next line of code.
3. `@ts-ignore` directive is used to ignore errors on the next line of code.
4. `@ts-nocheck` directive is used to disable all type checking for the next
   line of code.
5. `as any` directive is used to tell TypeScript that you know what you're
   doing.

## When to bypass type checking

1. When you know more about the runtime code than TypeScript does.
2. When you know more about the type than TypeScript does.
3. When you don't understand the error -- Try refactoring your runtime code.

```ts
const staticObj = {
	a: 1,
	b: 2,
};

const dynamicObj = {};
dynamicObj.a = 1;

// Property 'a' does not exist on type '{}'.

formatCurrency(data.transactionAmount as unknown as number);
```
