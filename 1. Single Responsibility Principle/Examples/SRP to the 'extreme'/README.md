# SRP to the 'extreme'
## Context
The Single Responsibility Principle is a principle we should always comply to. However, just like with every other SOLID principle, taking it to the 'extreme' would decrease maintainability of our code. What is meant by 'extreme', is that we have single function classes. Single function classes would mean we comply in its most extreme 'form' to : 'Each software module should have on and only one reason to change'. In the example below we will find out the down-side of applying the principle in its most extreme form.

## Example

Let's analyze the following **Pump** class:

<img src=Pump.png width=50% height=50%>

The functions within the class have a 'high' level of cohesion. This statement is based on three arguments:

 1. It is assumed that all the client(s) of this class will need this 'group' of functions when controlling the **Pump** class.
 2. Tight coupling will arise in case the **Pump** class's internals will be seperated amongst multiple classes. Tight coupling is often an indication that classes need to be reorganized and grouped together.
 3. The 'group' of functions form a single purpose, namely maintaining the state of the **Pump**. Each function in the **Pump** class works with the same property: 'state'.  

The client of the **Pump** class will look like so:

<img src=PumpClient.png width=20% height=40%> 

 What will happen if we 'over-engineer' here?

 We could have four separate **Pump** responsibilities, namely:
 
 1. **PumpStateValidator**, responsible for boundary checking of the state
 2. **PumpState**, responsible for maintaining the pump state
 3. **RunningPump**, responsible for keeping the pump in constant speed
 4. **StoppingPump**, responsbile for stopping the pump

 This is how it would look like if we refactor the **Pump** implementation like described above:
 
<img src=PumpState.png width=50% height=50%> 
<img src=PumpRunningStopping.png width=40% height=50%> 

The above code complies to the SRP, even more then our first implementation. However, this implementation comes with a cost.

The maintainability and reusability of our code is negatively impacted. Let's ellobrate on that statement: 
1) Maintainability & Scalability: our code becomes more scattered. This will make it harder to comprehend and navigate through the code, because each class becomes like a puzzle piece and we have to find them together and try to see the whole. E.g. it will be hard to see how and where **PumpState** is used and updated. Negative impact on the understandability of the code will result in more time spent and an increase in bugs.  
2) Reusability: instantiating **PumpClient** will require us to create 3 dependencies:
<img src=PumpClient3Dependencies.png width=20% height=30%>  
This is time consuming for both production and test code, because each dependency must be created. You can imagine the depedency 'hell' on large scale. In addition, as mentioned in Maintainability & Scalability, the code will be harder to comprohend, this makes reusability harder and can result in wrong use.

## Conclusion

As seen in this example, taking SRP to the 'extreme', will result in a decrease in readability and negativily impacts the coding effort. A decrease in readability arises because of the code being more scattered. Negative impact in the coding effort arises because we will create a more complex dependency model. These disadvantages will result in more time spent and an increase in bugs, because the overall system will be harder to understand.

## Extra

One of the most difficult aspects of the Single Responsibility principle is to find the optimum between 'extreme' and 'violation'. There is no 'black' or 'white' answer to that, because it depents. I ask my-self the following questions to come as close as possible to the Single Responsibility optimum:
1) Are the functions of the class maintaining a single state?
2) Does thight coupling arise when seperating the class in multiple responsibilities? 
3) Do the 'using' clients 'almost' always need the group of public functions to influence the state of my class?
4) Is readability negativily impacted in case I further split-up the responsbility of this class?
