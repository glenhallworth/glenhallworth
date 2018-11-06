Title: React Ecosystem is a Minefield
Published: 11/6/2018
Tags: ["React", "Single Page Application", "SPA", "Javascript"]

---

This year I have been working on a React project. I've been enjoying it. I can't help feel that it should be easier to use. It's like a minefield. 

React as a single page application technology is simple. 

- It's just javascript, including JSX
- Focuses narrowly on the UI layer
- External changes come from props, internal changes from state

React is a niche library for building user interfaces. It's good at that, and it's an enjoyable development experience. React encourages you to treat the UI as the rest of your code because it is just javascript. However you don't get batteries included. Single page applications are not just the UI, they usually also involve

- AJAX
- Forms and validation
- Authentication
- Routing and navigation
- And more...

**React makes it hard to fall into the [Pit of Success](https://blog.codinghorror.com/falling-into-the-pit-of-success/).** 

> The Pit of Success: in stark contrast to a summit, a peak, or a journey across a desert to find victory through many trials and surprises, we want our customers to simply fall into winning practices by using our platform and frameworks. To the extent that we make it easy to get into trouble we fail.

If you require routing in your application, React offers a [page with links to 10 different libraries](https://reactjs.org/community/routing.html). If you contrast this to Angular, it [links to one](https://angular.io/guide/router), with documentation, examples and supported by the Angular team. 

The result is adding basic functionality into your React app is a mountain climb finding the right library for the job. There's quite a lot of work that goes into evaluating libraries. 

-  When was the last release? How often are new versions released?
-  [Bus factor](https://en.wikipedia.org/wiki/Bus_factor) - How many contributors. Does a major company support it.
-  Documentation
-  Examples
-  If I have a problem, question or need help can I get it? Github issues? StackOverflow? Blog posts?
-  Tests - I don't want regressions on new versions.
-  Downloads on NPM
-  Github stars
-  Does it support Typescript - Written in typescript, typings included in projects, typings from definitely typed or no support at all?  
-  Spiking out a prototype
-  And does it solve the problem...

You finally choose a library. It solves your problem. Months go by, and you update it occasionally, there's a breaking change you fix. Perhaps it is not as performant as you would like. Perhaps you open Github one day and see this

![](image/React-Ecosystem-is-a-Minefield/deprecated.png)

You curse. Make a mental note of technical debt to pay down. Think of [The Prime Directive](http://retrospectivewiki.org/index.php?title=The_Prime_Directive)

> Regardless of what we discover, we understand and truly believe that everyone did the best job they could, given what they knew at the time, their skills and abilities, the resources available, and the situation at hand.

It is not easy to build a large, complex single page application in React. It's hard to know if you've made the right choices. Making the wrong ones is easy. There is a lot to consider when building a React-based application. I wish it were safer. It's dangerous in this ecosystem. 