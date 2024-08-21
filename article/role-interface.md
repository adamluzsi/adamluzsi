# From Legacy to Liberty: Paving the Way for Maintainability

This article is crafted for Software Engineers, with explanations designed for all experience levels.

## The High Cost of Haste
--------------------------------------

We've all been there: under pressure to deliver quickly, we take shortcuts and sacrifice code quality for the sake of meeting deadlines. But what happens when this approach becomes the norm? I recall a company that exemplified this strategy, where the pursuit of speed led to an ever-growing mountain of technical debt. Every project became a "green field" – a euphemism for starting from scratch, not because it was the best approach, but because nobody dared touch the existing codebase.

This might seem like a recipe for rapid progress, but in reality, the opposite is true. As technical debt accumulates, productivity slows down dramatically. What initially seemed like a shortcut becomes an obstacle course of brittle code, tight coupling, and hidden dependencies. The company's developers were essentially trapped in a never-ending cycle of firefighting and patchwork repairs.

So, how can we avoid this fate? How do we strive for maintainable code that's easy to understand and modify? One key principle is to decouple our domain logic from implementation details – but what does that mean, and how do we achieve it?

## The Problem with Leaky Abstractions
--------------------------------------

Leaky abstraction occurs when a system reveals unnecessary details like a driver needing to know how to shift gears in a car. 
Ideally, the driver should just focus on driving, not the car’s inner workings. 

These leaks can subtly intertwine implementation details with business logic, 
making them seem harmless at first, but over time, 
they create tight couplings that make it difficult to adapt to changes.
This can severely limit the flexibility and maintainability of your application over time.

### The Story of the Leaky Database Integration

Consider a scenario where your application uses a relational database to store customer data. Your business logic depends on this database for fetching and updating customer information. You might start using tools like Object-Relational Mappers (ORMs) or SQL query builders, which at first seem convenient. However, as your application grows, these queries become deeply embedded in your business logic, leading to assumptions that the database will always be relational, with specific table structures and data-fetching methods. 

Testing your business code becomes slow and cumbersome, and you might be tempted to start using mocks and stubs to speed up the "business code" related unit tests.

Now, imagine you need to update your database to a new version with subtle behavioural differences in its query API. While the queries might still function, the results may no longer align with your business logic or the expectations set by your tests. If you're unfortunate and these bugs are minor enough to slip through your acceptance tests, the speed you gained by using mocks will have left you unaware of these hidden issues.

With significant effort, and help from your QA team and end-users, you manage to navigate these challenges. 

Just as your team finishes celebrating this achievement, a tech-savvy colleague suggests switching to a search-oriented database to improve search performance. The sales pitch is convincing, and everyone is excited about the change.

Suddenly, all those SQL or ORM queries embedded in your business logic become a massive liability. You can't just integrate this new solution; you need to analyse and rewrite large portions of your business layer, which has been built over the years on implicit assumptions about how the database works. This situation forces you to rewrite a lot of code, but it's all right because it is promoted as "The new Greenfield Project" at your company. But it doesn't quite feel like starting a project from scratch, does it?

Eventually, the team realised that achieving full feature parity with the new technology wasn't feasible in time. Instead, they settle for a scaled-down version. In an attempt to make the most of the situation, the anxious marketing team rebrands it for end-users as "The new Search Experience with a Minimalist Style for better results". Interestingly, having solid CSS skills can significantly enhance the appeal of a partially completed rewrite.

## From Assumptions to Expectations
-----------------------------------

So, what if we could turn these assumptions into explicit expectations? 
One approach is to define our dependencies as roles and clearly outline our expectations of their behaviour.

For instance, instead of working with a “database”, we could talk about a “repository” that retrieves our domain data (entities) by providing search-related interactions.

### Defining Roles with Interfaces

When building domain logic, it's tempting to jump straight into implementing a solution.
It’s often better to start by focusing on the domain layer,
clearly defining our business needs and data.

Instead, let's focus on defining our business needs and the data involved in our domain layer. 
As we do this, we'll encounter various resources that need to be interacted with – databases, file systems, or APIs.

Rather than choosing a specific technology or implementation at this stage, 
we should define the role these resources will play in our domain logic. 
This is where interfaces come in – specifically, role interfaces.

A role interface is a set of methods that meet the requirements of our domain. 
By defining these interfaces, we can keep our domain code focused on its core responsibilities.

As we write domain code, we'll inevitably make assumptions about how these methods will behave. 
We should document these assumptions within the role interface itself.

By defining these role interfaces, 
we create a clear understanding of what's expected from any resource that implements this interface. 
However, it's essential to note that an interface alone doesn't guarantee correct behaviour,
it only ensures syntactical correctness, such as function signature.

### Crafting Expectations From Assumptions: Consumer-Driven Contract Testing

With our role interface defined, we’ve outlined how the implementation should behave. 
Next, we turn these ideas into clear expectations by writing executable tests. 
But instead of testing the real implementation directly, our test uses the role interface itself.

This might seem counterintuitive at first – why test against an interface rather than the actual implementation?
Wouldn't it make it more difficult to make test arrangements if we don't have access to the test to the implementation itself?
Wouldn't it be easier to have the tests alongside the implementation itself where they will be used?

There's a crucial benefit to separating the tests from the implementation. By using the role interface in our tests, we're forced to think about what arrangements are necessary to meet our business requirements, without being influenced by the implementation details.

Because we don't have access to the implementation, we're more likely to identify exactly what's needed to make our test work. We can list these arrangement requirements as part of the contract, making it clear what's expected from any implementation that implements the role interface.

This approach inverts the dependency between the domain layer and implementations. 

## Realistic Testing Made Easy: How Contracts and Roles Enable Fake Doubles
---------------------------------------------------------------------------

Consumer-driven contract testing and role interfaces are excellent tools for keeping your domain logic separate from implementation details. But there’s more—they also offer a powerful way to enhance unit testing.

By using them, you can create fake testing doubles that replicate the real behaviour of your dependencies. 
This allows you to test your domain code with confidence, in isolation, and in-memory.

The best part? Fakes let you build a realistic, production-like environment within your tests. 
This makes your tests faster, more reliable, and easy to set up for various scenarios.

Interested in discovering how contracts and roles can help you harness the power of fake testing doubles? 
Keep an eye out for an upcoming article that delves into this topic!
