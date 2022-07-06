# A 'simple' example

## Context

The Open Closed Principle is a principle we should NOT always comply to. We should apply OCP in case we expect the class to extend, otherwise a less flexible solution might also fit. Extendability comes with a price, because we need to introduce abstractions. Abstractions generate indirections and indirections have negative impact on navigating and debugging the code. On the other hand, OCP provides us a way to leave existing code un-touched. This has positive impact on both our production and test code. There is less chance that we break existing functionality, because we only have to plug-in our new code in an existing architecture. Not touching existing code means we do not have to modify existing test cases and re-test. 

## Calculator

Let's analyze the following **Calculator** class:

<img src=Calculator.png width=50% height=50%> 

A simple calculator class which can do calculations on two doubles. Let's write two tests cases to verify some behavior of our calculator:

<img src=InvalidActionTestCase.png width=70% height=70%> 
<img src=AddActionTestCase.png width=60% height=60%> 

The tests run succesfully. 

The customer requests us to extend the **Calculator** class with an extra operation, namely the 'Substract' operation. We add this new operation to the actions and extend the functionality:

<img src=SubstractAdded.png width=50% height=50%>

This violates the Open Closed Principle, because we modified existing code. As the principle states: 'A module should be open for extension, but closed for modification'.

Why is it so harmfull to modify existing code?

Let's execute our test code again to verify that our function is still working as expected. 

<img src=FailedTest.png width=80% height=80%>
<img src=FailedTest2.png width=50% height=50%>

Our test code failed! As you can see modifying existing code has negative impact on our existing, stable code and tests. The first failure arose because the new action: **Action::Substract** is 'aliasing' the number we used for our exception case. Now we have to refactor our tests and re-test! The second failure arose because we accidently removed the 'break' after the **Action::Add** action. This was luckely covered by a test case, so we could quickly identify the root cause. Imagine if this 'add' action was not tested, because we assumed that the original author of the code had proper test cases in place. This would cause a very nasty bug in the code, because each time we would perform the Add instruction we will end up with a Substraction. 

Is this implementation really wrong? Not looking at SRP on the functional level, I would say it depents. From my point of view it depents on the following factors:
1.  The 'change-frequency' of the implementation. Will it change more then once?
2.  The coupling the implementation has with other parts of the code and test cases. How much 'impact' will changing this class have on my test cases and other parts of the system?
3.  The complexity of the implementation. How complex is it to extend the class by changing it? Can we clearly oversee what happens when we change the class?

Let's say we know that this class will be changed frequently and a lot of test code is attached to it. 

How can we improve? 

Let's 're-play' the above scenario by applying the Open Closed principle. The operations will be defined in seperate classes:

<img src=CalculatorOperations.png width=40% height=50%>

The calculator class will look like so:

<img src=OCP_Calculator.png width=50% height=60%>

And the test cases:

<img src=OCP_Testcases.png width=50% height=60%>

Above tests pass. 

Let's add the 'Substract' operation:

<img src=SubstractOperation.png width=40% height=60%>

All the test cases still run succesfully:

<img src=PassingTests.png width=30% height=50%>

The exception test case still passes, because we are using a nullptr to provoke this behavior and not some random integer number. The second test case still passes as well, because we did not have a 'chance' to accidently break the existing code. The only 'part' where we could introduce a bug is in our new operation. 

In principle we should not have to re-run the test cases, because we did not modify existing code. Extending our code now means extending our test cases. 

This OCP approach ensures that we need less knowlegde about other parts of the system. We don't have to know about the other operations, but only about how **OCP_Calculator** uses our newly introduced operation. This increases our violocity in development.

Understandability is debatable. We reduced the cyclomatic complexity of the system, but we increased the amount of classes. A reduction in cyclomatic complexity improves readability of our code. An increase in the amount of classes will distribute the calculator implementation, which makes it harder to comprehend the overall intent.

## How about 'in between'?

Another approach could have been using seperate functions for each operation and still group them under the **Calculator** class (or namespace):

<img src=AlternativeSolution.png width=35% height=50%>

This solution ensures OCP on the functional level and still have the calculator operations in a single 'shell' to garuantee readability. Adding an operation would mean extending our calculator class with an additional function. Re-compilation of all 'using' clients will be required in case a function is added and our clients are depedent on the concrete implementation of the calculator. However, using an interfaces in between could resolve this issue. 

Let's say we have client 'A' using our initial calculator implementation:

<img src=InBetweenInitialImpl.png width=40% height=60%>

The customer asks use to extend the system with client 'B' that consumes the same calculator only with an additional Exponentiation operation. We can just introduce a new interface and add the operation to the existing implementation: 

<img src=InbetweenNewOperation.png width=50% height=70%>

It is chosen to reconstructe a complete new interface **ICaculatorExponentation**, because in my opinion the added value of readability is more important here. 

The introduction of a new interface means we don't modify our existing interface. This results in that our using clients, of the **ICalculator** interface, don't need to recompile or have to be adjusted in case mocks are based on this interface.


## Conclusion

I thought deeply and thoroughly about the Open Closed Principle. The essence is in the above example, but from the start you could tell that the example was not complying to the Single Responsibility principle. Complying to the SRP would directly have resulted in our final solution covered in: 'How about 'in between'? 

OCP states: 'A module should be open for extension, but closed for modification'. I would say that the principle mostly complies on the 'functional' level. We should not modify or remove the functions in a class, but we can add a function to a class without breaking our code. In my opinion OCP is about estimating what the impact will be on our production and test code when we extend it. We should not blindly comply to this principle not in production code nor on legacy code. Blindly complying to OCP will only result in complex code. In production OCP can always be applied later on, e.g. making a function virtual so we can call it in our override and add aditional behavior. In legacy code we can apply serveral design patterns (e.g. decorator). However, I do think we have to think carefully about single responsibility and dependency inversion. Complying to single responsbility and using dependency inversion (interfaces) will result easy extendable code.
