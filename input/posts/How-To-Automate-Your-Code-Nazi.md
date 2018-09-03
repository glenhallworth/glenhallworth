Title: How To Automate Your Code Nazi
Published: 9/3/2018
Tags: ["Testing", "Conventional"]

---

Do you have a colleague who doesn't follow your naming convention?

Are you sick of pointing out obvious mistakes in pull requests?

Did you forget to put [Authorize] attribute on the controller and left a security hole in your application?

Never fear, there is a solution to this problem. You can use convention tests to enforce rules automating your inner code Nazi.

### The Problem

The project was using a variety of collection types in DTO properties. It was a real mix bag, including IEnumerable, ICollection and Arrays. There were some concerns the team had.

- The lack of consistency, you had to look at what the return type was. Also when writing new queries, the developers had to think about what type they would use.
- The immutability of data structures.
- Concerns about lazy evaluation and desire to guarantee to consumers the collection had been iterated. [^1]

The team decided to use IReadOnlyCollection everywhere we needed a collection.

### The Solution

[Conventional](https://github.com/andrewabest/Conventional) is a library to help writing convention tests. It includes a lot of common scenarios. You can also write your own, which we will do here.

To create a convention specification, inherit from ConventionSpecification and override IsSatisfiedBy method.

This code relies heavily on reflection to examine the underlying types to determine whether the property is a collection and that collection is using IReadOnlyCollection.

<script src="https://gist.github.com/glenhallworth/583bc0f55ac159b2daeaf91586b6447e.js"></script>

The project used [MediatR](https://github.com/jbogard/MediatR) for all commands and queries. Meaning all queries implement IRequest<> interface. With assembly scanning it is easy to find all these queries.

<script src="https://gist.github.com/glenhallworth/9ba8e7d7d28068522540daed1f9b1f7a.js"></script>

What this code does is

1. Find all the return types of queries - that is the _T_ in IRequest<T>
2. Get all the property types of _T_ recursively.
3. Ensure these types conform to the convention specification DtosMustUseIReadOnlyCollectionForEnumerablesConventionSpecification

### Wrap Up

By writing a convention test, we solved the problem we had. We also

- Made it easy to refactor by finding all the places that weren't using IReadOnlyCollection
- Guaranteed we couldn't accidentally reintroduce the problem by having a convention test as part of our test suite

Convention tests are a great way to prevent mistakes and to ensure greater consistency in your codebase. Plus that inner code Nazi will be less annoying to your team.

[^1]:

  https://blogs.msdn.microsoft.com/ericwhite/2006/10/04/lazy-evaluation-and-in-contrast-eager-evaluation/
