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