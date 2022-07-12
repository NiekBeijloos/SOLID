# Relations between the SOLID principles

## Context
It is interesting to observe the relations between the SOLID principles, because implementing one can result in the other. Knowing these relations provide clearance in why certain principles exist. Some 'exist' more in the theoretical sense, others more in practical sense. The SOLID releations could help us to simplify adhering to them, because relations provide a link to stand-alone pieces that result in better remembering. 

## Relations
Each principle has a dependency to one another, except for the Single Responsibility Principle. The SRP is used by other principles, but is not dependent on other principles. I would say SRP exists more on the theoretical level. It contains a 'guideline' to develop high quality software, but it is really intangible when we talk about the 'how'. An expierenced software engineer understands the meaning of this abstract, intangible 'how', but it will be hard to demonstrate it to a beginner. Applying it correctly almost requires you to have a new 'sense'. 


<img src=Relations.png width=50% height=50%>

The open closed principle is dependent on the Liskov Substitution, Depedency Inversion and Single Responsibility principle. OCP is about preventing to break our working, existing code and test cases. The principle is (in my opinion) the most appicable on the functional level of the code and less on the class level. Just like Single Responsibility, OCP exists more in the theoratical sense. It is clear 'what' we want to achieve, but it is complex to explain how to exactly accomplish this. One way to accomplish 'Open Closed' is to use DIP. DIP emphasizes the use of abstractions (i.e.: interfaces and abstract classes) and these abstractions provide an easy way to extend our system with new functionality. The DIP is about change, OCP is about extension, which is a form of change. DIP is more the umbrella principle and the OCP resides under that umbrella to emphasize extension without modifying. DIP additionally simplifies replacement in case this is required. Replacement means removal of 'old' implementation and insertion of 'new' implementation. Depedency Inversion 'exists' in the practical sense. It has a strong and clear explanation on 'how' to achieve it, but the 'why' resides more in the Open Closed principle.

Liskov Subsitution is another principle 'existing' in the practical sense. It explains use 'how' we should use inheritance in a way that we can ensure that all our sub-types can be used just like our base types. We apply this principle to prevent bugs/undefined behavior when extension is required and to prevent complexity in a system. The more exceptional cases we have of our base contract the more our cyclomatic complexity will grow. Inheritance is a 'resource' to achieve Open Closed and Liskov Substitution provides a way to apply this 'resource' in a correct way. This explains the relation between LSP and OCP.

The Open Closed principle uses the Single Responsibility to simplify extension without modifying. 






I would say OCP has the strongest relation towards Dependency Inversion, but the intent of both principles diverges a bit. The DIP is about change, OCP is about extension, which is a form of change. DIP is more the umbrella principle and the OCP resides under that umbrella to emphasize extension. DIP emphasizes the use of abstractions (i.e.: interfaces and abstract classes) and these abstractions provide an easy way to extend our system with new functionality, but also to exclude it. 