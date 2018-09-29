Title: From the Trenches - 5 Lessons of a Successful Project
Published: 9/29/2018
Tags: ["Projects", "Agile", "Delivery"]

---

I have been working on building an enterprise application to replace Access databases, Excel and much paper. The organisation is well entrenched in their processes, using this system for several years. They had outgrown their current systems, limiting their ability to innovate and join the digital revolution. I joined after a previous team had tried and failed to replace the system, costing the company money in the process. These are just five lessons that helped us succeed. 

## Coaching Agile

It is easy to forget after years of being agile in software development that most organisations and most departments outside of I.T. are not familiar with agile practices. It takes time to learn. There are new ideas and ceremonies to understand. Given this is a large project with multiple subdomains and product owners, within addition several stakeholders there were quite a few people to bring along to the journey. 

During the development, when adding further subdomains into the project we were essentially starting over the agile journey with new stakeholders and product owners. The whole project we've been starting from zero, it's been important to challenge our assumptions constantly and educate our colleagues in our way of working. Coming in after a failed project, coaching agile to stakeholders has increased their confidence in us and further ensures the success of the project. 

## Get the Architecture Right

In a complex enterprise system of this size and project length the ability to change and to reason overtime is required to deliver and adapt. From the start, we've implemented Domain-Driven Design to inform our model and tame complexity in the monolith. We've also used CQRS to separate our writes and reads, simplifying our software. 

The right architecture flows throughout the whole project. It allows you to understand how the domain works, to add new features quickly and test your code effectively. Preventing the system from going to spaghetti expands the useful lifespan of the application. It allows you to iterate towards an application fit for purpose. 

## Just Enough Testing

In the long run, adequate testing saves time, preventing defects and reducing risk making changes. There are many different types of testing and testing strategies. We decided to maximise the bang for our buck using two kinds of testing.

**Integration Testing** - Performing subcutaneous tests by calling the API, executing a command, saving to the database and verifying the results through calling the API, running a query and reading the database provides a good level of confidence the system works. Using the system to test the system limits the number of tests you need to write to ensure correctness. 

There are tradeoffs to integration testing. They are typically slower than unit tests. However, with the right architecture and picking the most valuable slice of application to test you prevent significant problems in your application. 

**Convention Testing** - Enforcing agreed upon standards across the project and preventing developers from making mistakes. Convention tests address specific types of [issues](/How-To-Automate-Your-Code-Nazi) traditional testing does not pick up. They provide enormous value for little effort in writing testing your whole code base for problems. 

These were not the only types of testing we performed. The critical idea was focusing on tests that maximised value for minimal effort. 

## Be Pragmatic

In all projects, including large projects such as this one, you need to make pragmatic decisions. There are competing stories, bugs and technical debt to pay down to prioritise. You need to take a practical approach and be pragmatic to the priorities. 

Although the vast majority of choices we made with third-party libraries were correct, we made mistakes. The forms library worked well 90% of the time. It did not have great integration with our language choice and in specific scenarios performance was terrible. Midway through the project the maintainers deprecated the library and started a new library. It is significant work to replace the library, but that would be a distraction, the library still works, we can work around the 10% of issues. We are making pragmatic decision to take on technical debt and focus on what is more important. 

## Deliver

Building trust with organisation and stakeholders was necessary, we were coming in after a failed project. We were agile. They were a traditional organisation. It was reasonable for them to be sceptical of us. To achieve the goals of the project and overcome trust chasm we delivered.

We built a minimum-viable product, we had users, using the software in production as soon as we could. 

We frequently deployed, without breaking changes, often. Soon after demonstrating features, our users could use them, in production, for real. 

When we received feedback, we took it on board, investigated the underlying problem and came up with a solution. If it did not work, we iterated until we found a suitable solution. 

Over time we gained the trust of the organisation by delivering. It is satisfying to have someone in the organisation to sell the project to their colleagues, to explain the benefits without being prompted. Delivery is achieving success. 