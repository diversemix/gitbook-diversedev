---
description: >-
  A walk-through improving an example "Message Store" to demonstrate SOLID
  principles.
---

# SOLID in TypeScript

## Introduction

This course has been written to try and encapsulate what we think is a good approach to software design and implementation on a number of open source projects we are about to undertake. There is the assumption that you will know and have used JavaScript and TypeScript. 

The aim of this course is to walk you through the reasons why certain approaches are taken to improve the code the we will be using. Hopefully you will find this useful and it will help writing code with other developers. None of it is new and all of it is adopted as good practice.

The first lesson, "Lesson Zero" is there just to show you around the example code. This is then followed by two main sections; the first section are lessons one, two and three which are general principles before the section each lesson based on the 5 SOLID principles.

One caveat... This repository has been built to make it easy to follow the lessons. It's not an example of a production setup or environment.

## Lesson Zero

{% hint style="info" %}
AIM: Understand the example you will be using throughout these lessons

[Either use your browser to navigate the code or clone the repository](https://github.com/diversemix/solid)
{% endhint %}

* Take a look at the `message-store/src` folder, start by looking at the interface in `types.ts`
* See if you can guess the implementation just by looking at the interfaces.
* Look at the implementation in `message-store.ts`
* Looking at the sample in `index.ts` see if you can guess the output.
* Now run the example by executing `make run` from inside the `message-store` folder.

It would be good to discuss and/or list out the improvements that could be made to this code and why. We are going to go through improving this code throughout the lessons and hopefully you will 

## Lesson One

{% hint style="info" %}
AIM: Understanding [Command-Query separation](https://en.wikipedia.org/wiki/Command%E2%80%93query_separation) as a principle making code easier to read and comprehend.

This is _**NOT**_ CQRS
{% endhint %}

{% embed url="https://en.wikipedia.org/wiki/Command%E2%80%93query\_separation" %}

Writing code that is readable and maintainable is learnt early on in our career, either after we have to maintain our own code - or after we hear other developers talking about ours. It's not only good manners it's essential to keeping code functional and easy to fix, and we all want to be that developer. 

There are many rules and principle to use to keep our code readable, like using well named variables and functions. But we want to take this to the next stage and understand how to write code with an architecture that means it's easy to navigate and comprehend. We certainly have come across code that is the opposite, huge functions and files which to the outsider are almost impenetrable.

* Let's look at our `MessageStoreImplementation` class in `src/message-store/message-store.ts`
* There is a repeated line in both the `save()` and the `read()` methods.

```text
const file = path.join(this.dir, id + '.txt');
```

When you looked at this code you may have noticed this repetition and wanted to factor out this into its own method as this is a potential source for bugs.

* Let's create a new function with the signature `getFilename(id: number) : string`
* Ensure that the implementation of the method uses the common line \(above\) and returns the result.

Notice the signature of the function we have just made. This is typical for a query in CQS, a query returns the type of things you are searching for and takes a number of argument required to find it. The most important thing is that its [idempotent](https://en.wikipedia.org/wiki/Idempotence), meaning you can call it as often as you like without changing or creating any side effects.

* Looking at the `read()` function in the file - does this have any side effects?

This is sort of tricky, one thing you might have noticed is that it doesn't have a typical signature for a query - it does not return the message. Another thing is the `EventHandler`. This is used to return the result but we cannot guarantee that it won't have side effects that are written by the consumer of the event. So let's remove that too.

* Change `read()` to conform to the signature 
* Remove the `EventHandler` 
* Change `index.js` and check `make run` still works.

So that was the query side of CQS, now for the "command". You can probably guess that a command should have a function signature that returns void and takes in any necessary arguments. In our example `save()` should have such a signature.

* Change the `save()` function to not return anything.

If you havn't already done so...

* Change the `save()` and `read()` functions to use the `getFilename()` that you added earlier.

**NOTE**: Its ok that command functions call query functions, but not the other way round. 

Implementing code like this will make the code a lot easier to read, as functions can be easily classed as queries or functions depending on their signature, so you don't need to wade through the implementation. Agreeing to write code with this in mind can help enormously on larger projects.

## Lesson Two

{% hint style="info" %}
Learn to write code that is easier to write using a "fluent interface". Ironically this breaks the "command" signature you have just learnt, so you will also learn that these are principles not rules `:)`
{% endhint %}

{% embed url="https://en.wikipedia.org/wiki/Fluent\_interface\#JavaScript" %}

## Lesson Three

tolerance

{% embed url="https://en.wikipedia.org/wiki/Robustness\_principle" %}

## Lesson Four: Separation 

## Lesson Five: Open/Closed

## Lesson Six: Liskov

## Lesson Seven: Interface Segregation

## Lesson Eight: Dependency Inversion  

## Further Reading / Inspiration

| Description | Media Type |
| :--- | :--- |
| [Encapsulation & SOLID](https://app.pluralsight.com/courses/55b3efd7-1363-46d7-8542-1c9a100502fe/table-of-contents) | Pluralsight Course |
| [Monads & Gonads](https://www.youtube.com/watch?v=b0EF0VTs9Dc) | YouTube Video |
| [SOLID](https://en.wikipedia.org/wiki/SOLID) | Wikipedia |
| [SOLID Object-Oriented Design by Sandi Metz](https://www.youtube.com/watch?v=v-2yFMzxqwU) | YouTube Video |







