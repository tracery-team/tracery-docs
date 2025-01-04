# Code Style

Here is the guide how to properly write
your TypeScript code. It doesn't cover
the best practises of libraries or tools.
To learn more about, let's say, React, go to
[the proper](library-react.md) section.

## PascalCase is the way
**_Always_** use 1. PascalCase for classes or 2. pascalCase for everything else
```ts
class HumanManager { // Here PascalCase
  constructor (
    private readonly name: string,
    private readonly surname: string,
  ) {}

  get fullName(): string { // ...and here pascalCase
    return `${this.name} ${this.surname}`
  }
}

// Here, again, pascalCase
const reallyLongIdentifier = new HumanManager('Mateusz', 'Nowak')
```

## Tab size is 2 spaces
You will deal with deeply nested JSX tags, like:
```tsx
<Container>
  <Section>
    <AnotherSection>
      <SomeComponent />
    </AnotherSection>
  </Section>
</Container>
```
having tab size being equal to 4 spaces
makes your code less readable. Tab size of 2
makes code more compact, therefore much
more readable

## Don't use semicolons (unless really needed)
```ts
const a = 1
const b = 2
console.log(a + b)
```
TypeScript compiles down to JavaScript with
automatically inserted semicolons. In some **_rare_** cases,
however, they are necessary, and only then they should
be inserted. If you see red errors around [] or (),
try inserting a semicolon

## Try not to omit brackets in compound statements
```ts
// Bad
if (something)
  doSomething()

// Good
if (something) {
  doSomething()
}
```

## Embrace single quotes:
```ts
import { React } from 'react'
import { css } from 'styled-components'
```
They are faster to type. It will be a standard
preference for our codebase. Use double quotes
only when the string contains a single quote:
```ts
const quotedString = "'Mateusz'"
```

## Don't write long lines. Split them!
```tsx
// Bad
const measurement = measureVelocity(valueA, valueB, valueC, valueD, valueE)

// Good
const measurement = measureVelocity(
  valueA,
  valueB,
  valueC,
  valueD,
  valueE,
)

// Bad
<MyComponent propA={{ ...something }} propB={10.0} propC />

// Good
<MyComponent
  propA={
    ...something,
  }}
  propB={10.0}
  propC
/>
```

## Use trailing commas
JavaScript allows you to have
trailing commas in function calls,
objects and arrays
```ts
const schema = {
  type: 'post',
  payload: {
    name: 'Mateusz',
    surname: 'Nowak', // here is trailing comma
  }, // here is trailing comma
}
```

## Use arrow functions
Arrow functions' behavior is more predictable
(I am primarily talking about [this](https://stackoverflow.com/questions/33308121/can-you-bind-this-in-an-arrow-function)).
Therefore, when possible, you should use them:
```tsx

// an arrow function
const App = () => {
  return (
    <main>
      <h1>Hello, world!</h1>
    </main>
  )
}

// another arrow function
const add = (a: number, b: number) => a + b

// you can use them as methods in classes
class Person {
  constructor (
    public readonly name: string,
    public readonly surname: string,
  ) {}

  greet = (friend: Person) => {
    const greeting = `Hi, ${friend.name}, I am ${this.name}!`
    console.log(greeting)
  }
}

```

## Allow TypeScript to infer types when possible
TypeScript type inference works well most of the times.
In fact, when type inference is possible, TypeScript will
definitely infer types better than you. So, use types
only when needed (normally for function arguments)

```ts
// Not needed - TypeScript already knows that a is a string
const a: string = 'Hello, world' // type of a is 'string'

const a = 'Hello, world!' // type of a is a literal 'Hello, world!'
                          // the type here is more narrow since TypeScript
                          // knows that a cannot be changed to
                          // any other string

// why do you need ": number" in the end?
const add = (a: number, b: number): number => a + b

// this one is better: TS already knows 
// that the function returns a number
const add = (a: number, b: number) => a + b
```

## Don't use any
Type 'any' kills the TypeScript type system.
The moment you introduce 'any' to your codebase,
you allow bad things to happen. Don't do that.

Consider reading on the internet how to:

1. Use 'unknown' type instead
2. Use generics
3. Use type guards with validation

Normally 'any' occurs when you get something
as a network response. Then, you need to validate
the response and prove that it has the desired
type using type guards

```ts
// Example of a type guard validation

type Person = {
  name: string
  surname: string
}

const isPerson = (maybePerson: unknown): maybePerson is Person => {
  if (maybePerson === undefined || maybePerson === null) {
    return false
  }
  if (typeof maybePerson !== 'object') {
    return false
  }
  if (typeof maybePerson?.name !== 'string' ||
      typeof maybePerson?.surname !== 'string) {
    return false
  }
  return true
}

// in some other place
const person = await fetchPerson() // type of person is any
if (isPerson(person)) {
  // Here TypeScript knows, 
  // that if isPerson returned true,
  // type of the person is Person
  console.log(person.name) // No error
}

```

## Don't use TypeScript enums and namespaces
Instead of enums you can use const enums.
Namespaces are just a language design mistake,
nobody uses them, so you shouldn't either

## Use TypeScript const enums
const enums are quite useful. They are similar to C macroses.
During compilation, they are turned into just JavaScript literals

```tsx
const enum Status {
  ON = 'On',
  OFF = 'Off',
  UNKNOWN = 'Unknown'
}

const status = Status.OFF // after compilation,
                          // it will be just const status = 'Off'

// You can skip literals in const enum definition,
// then TypeScript will use numbers 0, 1, 2, ...
const enum Flag {
  READ,
  WRITE,
}

const flag = Flag.READ // after compilation,
                       // const flag = 0
```

## Don't use interfaces, use types instead

>
>   **_Attention_!** Both types and interfaces do the same thing! </br>
>   In other languages there may be a distinction between those two.
>   TypeScript, however, is a language with duck typing, therefore
>   each type acts as an interface by default
>

In TypeScript you have two ways of declaring a type:


1. Interface declaration
2. Type declaration

```tsx
// preferred way
type User = {
  name: string
  surname: string
}

// Bad - don't use it
interface User {
  name: string
  surname: string
}
```

The reason behind this decision is
the fact that interfaces are quite limited,
and all their use-cases are covered with
type definitions.

## Ask about the rest
If you are unsure about something, just
ask me :)
