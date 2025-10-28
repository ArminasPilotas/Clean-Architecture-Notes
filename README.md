# Clean Architecture — Notes from Robert C. Martin

These are my personal notes and key takeaways while reading **_Clean Architecture_ by Robert C. Martin (Uncle Bob)**.  
The goal of these notes is to summarize the core ideas and principles of software architecture that help build systems which are easy to maintain, extend, and understand.

---

**PART 1. Introduction**

**Chapter 1. What is Design and Architecture?**

The goal of software architecture is to minimize the human resources required to build and maintain the system.

The measure of design quality is the effort required to meet customer needs.  
If that effort remains low throughout the lifetime of the system, the design is good.  
If that effort grows with each new release, the design is bad — it’s as simple as that.

Good software architecture reduces costs (it should not increase them exponentially).  
Similarly, if the architecture is poor, developers will struggle to deliver the same results with the same effort over time due to rising complexity.

Software should be “soft” — not just fulfilling requirements from stakeholders, but also being easy to change.  
We shouldn’t just build software to meet requirements and fix bugs; we should design it for change.

Eisenhower matrix by priority:
1. Urgent and important  
2. Not urgent and important  
3. Urgent and not important  
4. Not urgent and not important

---

**Chapter 2. A Tale of Two Values**

Start of programming languages:  
Turing (binary) → Assembly (converted to binary) → Compilers with new languages like COBOL, PL/1, SNOBOL, C, PASCAL, C++, JAVA…

---

**PART 2. Starting with the Bricks: Programming Paradigms**

**Chapter 3. Paradigm Overview**

Programming paradigms: structured programming, object-oriented programming, functional programming.

- **Structured programming** — replaced `goto` statements with control structures like `if/then/else`, `do/while/until`.  
- **Object-oriented programming** — evolved from ALGOL; introduced heap-based memory and classes with instance variables and methods (encapsulation and polymorphism).  
- **Functional programming** — introduced immutability in languages like LISP, where symbol values do not change.

No new programming paradigms have been added since 1958–1968.

Architecture concerns: function, separation of components, and data management.

---

**Chapter 4. Structured Programming**

Structured programming was invented by Dijkstra when programs were written in binary or crude assembly and required hours to compile and test.

Dijkstra proved that all programs can be constructed from just three structures: **sequence, selection, and iteration** — the foundation of structured programming.

Structured programming allows recursive decomposition of programs into small, provable units.

It forces us to decompose a program into a set of small, testable, and understandable functions.

---

**Chapter 5. Object-Oriented Programming**

Object-oriented programming combines **data and behavior**.

Nygaard moved the function call stack frame to the heap and invented object-oriented programming.

OO helps model the real world, making software easier to understand through **encapsulation, inheritance, and polymorphism**.

OO languages allow encapsulation of data and functions — using `private`/`public` modifiers.

- **Inheritance**: redeclaring variables and functions within an enclosing scope.
- **Polymorphism**: allows values or types to assume different forms — e.g., UNIX device drivers sharing `open`, `close`, `read`, `write`, `seek`.

**Dependency inversion** allows components like UI, Database, and Business Logic to act as independent plugins.  
When one changes, others don’t need to be redeployed — enabling **independent deployability**.

---

**Chapter 6. Functional Programming**

Variables in functional programming are **immutable** — once assigned, they don’t change.

Why immutability? Because mutable state is a major source of bugs: race conditions, shared-state concurrency issues, and unintended side effects.

---

**PART 3. Design Principles**

The **SOLID** principles guide how to structure functions and data into classes and how those classes should interact.

Their goals:
1. Tolerate change  
2. Be easy to understand  
3. Enable components that can be reused across systems

---

**Chapter 7. SRP — The Single Responsibility Principle**

SRP: The best structure for a software system is influenced by the social structure of the organization that builds it.  
Each module should have one, and only one, reason to change.

A module should be responsible to one, and only one, actor.

**Example:**  
`Employee` class with methods `calculatePay`, `reportHours`, `save` violates SRP:
- `calculatePay` → Accounting  
- `reportHours` → HR  
- `save` → Database Admin  

To fix this, separate the responsibilities:
- `PayCalculator`  
- `HourReporter`  
- `EmployeeSaver`  

Each consumes data from `Employee`.  
Alternatively, use the **Facade** design pattern.

SRP tells us to separate code based on different actors’ responsibilities.

---

**Chapter 8. OCP — The Open/Closed Principle**

A software artifact should be **open for extension but closed for modification**.  
We should make systems easy to extend without a high cost of change.

This is achieved by partitioning systems into components arranged in a dependency hierarchy, protecting higher-level components from changes in lower-level ones.

---

**Chapter 9. LSP — The Liskov Substitution Principle**

If a program can use a parent class, it should also work correctly with any of its child classes.  
A child class must be substitutable for its parent without introducing bugs or unexpected behavior.

---

**Chapter 10. ISP — The Interface Segregation Principle**

A client should not depend on methods it does not use.  
Instead of large, “fat” interfaces, we should create smaller, more specific ones tailored to each client’s needs.

---

**Chapter 11. DIP — The Dependency Inversion Principle**

Flexible systems depend on **abstractions, not concretions**.

Two key ideas:
1. High-level modules should not depend on low-level modules.  
2. Both should depend on abstractions.

Every change to an abstract interface implies a change in its concrete implementations.

Guidelines:
- Don’t reference volatile concrete classes.  
- Don’t derive from volatile concrete classes.  
- Don’t override concrete functions.

---

**PART 4. Component Principles**

**Chapter 12. Components**

Components are units of deployment — the smallest entities that can be deployed as part of a system.  
They may appear as `.jar`, `.dll`, or `.exe` files, depending on the language.

Originally, programmers separated function libraries from applications to shorten compile times.  
Loaders could then load libraries and applications independently.

---

**Chapter 13. Component Cohesion**

Component cohesion principles:
1. **REP — Reuse/Release Equivalence Principle**  
2. **CCP — Common Closure Principle**  
3. **CRP — Common Reuse Principle**

**REP:** The unit of reuse must be the same as the unit of release.  
Reusable components must be versioned, packaged, and tracked through a release process.

**CCP:** Classes that change for the same reasons and at the same times should be grouped together.  
Classes that change for different reasons should be separated.

**CRP:** Classes and modules reused together should be in the same component.  
This avoids depending on code you don’t need.

---

**Chapter 14. Component Coupling**

**The Acyclic Dependencies Principle** It mentions about elemeninating dependency cycles by working on releasable components and when component is working and ready to deploy, it should be released for use by the other developers. Acyclic dependencies principle requires the dependency graph of components in a system to have no cycles adhering to this principle makes systems more stable, independently deployable, and easier to understand and maintain.

**Top-Down Design** Component structure cannot be designed from the top down, when system evolves and system grows or changes it will be hardly maintainable.

**The Stable Dependencies Principle** A component should be as abstract as it is stable. Software components should be designed so that dependencies point from volatile (less stable) components to stable (more stable) ones. This ensures that a change in a volatile component does not force a change in a stable component, which is often depended upon by many other parts of the system.

---

**PART 5. Architecture**

**Chapter 15. What Is Architecture?**

The primary purpose of architecture is to support the life cycle of the system. Good architecture makes the system easy to understand, easy to develop, easy to maintain, and easy to deploy. The ultimate goal is to minimize the lifetime cost of the system and to maximize programmer productivity.

Architecture from the code side constructs from these points: Development, Deployment, Operation, Maintenance.

A software system that is hard to develop is not likely to have a long and healthy lifetime. So the architecture of a system should make that system easy to develop, for the team(s) who develop it.

To be effective, a software system must be deployable. The higher the cost of deployment, the less useful the system is. A goal of a software architecture, then, should be to make a system that can be easily deployed with a single action.

The impact of architecture on system operation tends to be less dramatic than impact of architecture on development, deployment, and maintenance. Almost any operational difficulty can be resolved by throwing more hardware at the system without drastically impacting the software architecture. But it doesn't mean that it's correct way to solve issue and problem probably lies in architecture decisions.

In early stages of development it is not necessary to choose database, web server, adopt REST or instantly use dependency injection frameworks. In early stages of development it's worth to keep any options open for future development.

---

**Chapter 16. Independence**

Good architecture must support: Use cases and operation of the system; The maintenance of the system; The development of the system; The deployment of the system.

Use Cases - Means that the architecture of the system must support the intent of the system.

Operation - System operation should support agreed throughput and response times for each use case that demands it.

Development - A system that must be developed by an organization with many teams and many concerns must have an architecture that facilitates independent actions by those teams, so that the teams do not interfere with each other during development.

Deployment - The architecture also plays a huge role in determining the ease with which the system is deployed. The goal is immediate deployment. A good architecture does not rely on dozens of little configuration scripts and property file tweaks.

Decoupling in architecture is a design strategy that minimizes the dependencies between components, allowing them to operate and evolve independently. It's also applicable for development teams, component deployments.

---

**Chapter 17. Boundaries: Drawing Lines**

Software architecture is the art of drawing lines - boundaries. Those boundaries separate software elements from one another, and restrict those on one side from knowing about those on the other.

You draw lines between things that matter and things that do not. For example UI doesn't matter to the database or database doesn't matter to the business rules.

Developers and customers often get confused about what the system is. They see the UI, and think that the UI is the system. Good example for this would be video game for customers experience matters the most like sounds, UI, mouse, keyboard but behind that interface there is a model like data structures, functions.

The plugin architecture was developed to create a collaborative software environment where an application can be created from the collection of different, reusable components that don't rely on one another but can still be assembled dynamically using these components.

---

**Chapter 18. Boundary Anatomy**

The architecture of a system is defined by a set of software components and the boundaries that separate them. Those boundaries come in many different forms.

Boundary crossing - Act of one component interacting with another, often by calling a function and passing data across a defined separation in the system. To create appropriate boundary crossing is to manage the source code dependencies.

The Dreaderd Monolith - It is simply a disciplined segregation of functions and data within a single processor and a single address space.

Deployment Components - Define the physical or logical separation between components that are deployed independently, such as DLLs or JAR files.

Threads - Both monoliths and deployment components can make use of threads. Threads are not architectural boundaries or units of deployment, but rather a way to organize the schedule and order of execution.

Local Processes - typically created from the command line or an equivalent system call. Local processes run in the same processor, or in the same set of processors within a multicore, but run in separate address spaces.

Services - The strongest boundary is a service. A service is a process, generally started from the command line or through an equivalent system call. Services do not depend on their physical location. Two communicating services may, or may not, operate in the same physical processor or multicore.

Boundaries in a system will often be a mixture (if it's not monolith) of local chatty boundaries and boundaries that are more concerned with latency.

---

**Chapter 19. Policy And Level**

Software systems are statements of policy. Indeed, at its core, that is all a computer program actually is. A computer program is a detailed description of the policy by which inputs are transformed into outputs.

Policy can be broken into many different smaller statements. Some of those statements will describe how particular business rules are to be calculated. Others will describe how certain reports are to be formatted. Still others will describe how input data are to be validated.

Level - the distance from the inputs and outputs. The farther a policy is from both input and output, the higher its level and vice versa the closer policy to input and output the lower level policy it is.

---

**Chapter 20. Business Rules**

Business rules - rules or procedures that make or save the business money, irrespective of whether they were implemeneted on a computer or they were executed manually.

Critical business rules example - bank changes X% interest for a loan it's critical business rule, if that rule doesn't exist in bank, it wouldn't probably exist.

Entities - object within our computer systen that embodies a small set of critical business rules operating on critical business data.

The business rules should remain pristine, unsullied by baser concerns such as the user interface or database used. Ideally, the code that represents the business rules should be the heart of the system

---

**Chapter 21. Screaming Architecture**

Screaming architecture - he idea is that the architecture of your system should immediately reveal its purpose. Just like looking at the blueprint of a building, you can tell whether it's a house, library, hospital. It should scream the domain / business intent / use-cases, rather than screaming which framework or database is used.

Good architectures are centered on use cases so that architects can safely describe the structures that support those use cases without committing to frameworks, tools, and environments.

If your system architecture is all about the use cases, and if you have kept your frameworks at arm’s length, then you should be able to unit-test all those use cases without any of the frameworks in place.

---

**Chapter 22. Clean Architecture**

Each architectures produces systems that have the following characteristics :
1. Independent of frameworks
2. Testable
3. Independent of the UI
4. Independent of the database
5. Independent of any external agency

In clean architecture source code dependencies must point only inward, toward higher-level policies.

The clean architecture circle layers from highest to lowest:

Entities - Entities encapsulate enterprise-wide Critical Business Rules. An entity can be an object with methods, or it can be a set of data structures and functions.

Use cases - The software in the use cases layer contains application-specific business rules. It encapsulates and implements all of the use cases of the system.

Interface adapters - set of adapters that convert data from the format most convenient for the use cases and entities, to the format most convenient for some external agency such as the database or the web.

Frameworks and drivers - generally composed of frameworks and tools such as the database and the web framework.

If you follow dependency rule it will save you a lot of headache going forward, when any of the external parts of the system become obsolete, such as database, or the web framework, you can replace those elements with a minimum effort.

---