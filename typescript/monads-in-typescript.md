---
description: Using monads to make your code more deterministic and readable
---

# Monads in Typescript

## Introduction

This small session just serves as an introduction to monads in typescript and why we use them. There are plenty of other more details and more interesting, one of my favorites is [Monads and Gonads](https://www.youtube.com/watch?v=b0EF0VTs9Dc) on YouTube by Douglas Crockford.

Any good developer in addition to wanting to write good functioning code will also have the aim to write expressive, easy-to-read code that can be appreciated by their co-workers especially when it comes to maintenance. Writing code that is easy to read and to reason about is essential on larger projects and is often a criticisms of javascript \(see [https://stackoverflow.com/questions/27632391/why-null-false-and-null-true-both-return-false](https://stackoverflow.com/questions/27632391/why-null-false-and-null-true-both-return-false)\), and if you want to really blow your mind take a look at [Javascript the Weird Parts](https://charlieharvey.org.uk/page/javascript_the_weird_parts).

So how do we write expressive domain oriented code that hides away all this Javascript weirdness? One way is by using monads to help. The following sections are a little explanation of the problems and how different monads can be used to help.

## Maybe Monad

Let's take a look at the following function signature:

```text
function getString() : string
```

This is perhaps as simple as it gets, there are no arguments and it returns a `string`- So we can safely say it looks like a query, and according to the Command Query Separation it should not be changing anything just returning a string and so pretty benign. 

However, we cannot really determine what it could return without looking into the implementation of the function. It might be possible it returns `undefined` or `null`. Then subsequent behavior might also change on the use of the [strictNullChecks](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html#--strictnullchecks) flag. So as you can see, even what appears to be a simple function that returns a string needs to be handled correctly for deterministic behavior.

We could fix this by returning an array which would be guaranteed to have zero or one element of the correct type. Then we would be more certain of what the function would return - it maybe nothing or a string. This is the `Maybe` or `Option` monad. 

So imagine that the implementation of the function looks something like this... in this case it simulates returning `null` but it could return `undefined` or a valid string.

```text
function getString() : string {
  let value : string = 'a string';
  value = null; // This line simulates something hiding the fact its assigned null
  return value;
}
```

Now let's use the `Option` monad to guarantee the return is either a string or empty.

```text
import { Option } from 'funfix';

function getString() : Option<string> {
  let value : string = 'a string';
  value = null;
  return Option.of(value);
}
```

If we use the monad implementation then because the return value is more deterministic we can simplify any code that uses it. So that we can use a string and be certain it is indeed a string.

```text
const guaranteedString = getString().getOrElse('default');
```

Or rather than using a default we can define a **L**azy function \(note that this does not strictly need to return a string in this case, or if could be used to perform an operation because the string was missing\).

```text
const guaranteedString = getString().getOrElseL<string>(() => getStringAnotherWay())
```

