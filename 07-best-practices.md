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
