# Violation

![Base](https://github.com/NiekBeijloos/SOLID/blob/master/3.%20Liskov%20Substitution%20Principle/Examples/History%20rule/Violation/Base.png?raw=true)

![Derived](https://github.com/NiekBeijloos/SOLID/blob/master/3.%20Liskov%20Substitution%20Principle/Examples/History%20rule/Violation/Derived.png?raw=true)

The above example violates the history rule of LSP. The base contract 'tells' us that the state of the device will not change in the **Initialize** function. However, the sub type (= Valve) additionally sets the state. From the base contract point of view no state change is expected after calling **Initialize**. Client code could be dependent on this contraint. This example is very wrong, but believe it or not, it does happen!


# Solution

To solve this problem we have to carefully think about the state first. Certain states, like 'Open' and 'Close' are very specific for a valve, but cannot be applied to a pump. So, why are we trying to fit these devices under the same base? What do they have in common? It seems that they have a lifecycle in common; Initialize, Uninitialize, etc. The lifecycle controls a generic part for all device, i.e. to enable/disable manual control. Let's rename the base class to **LifeCycleDevice** and seperate the valve state from the lifecylce state.

![Base](https://github.com/NiekBeijloos/SOLID/blob/master/3.%20Liskov%20Substitution%20Principle/Examples/History%20rule/Solution/Base.png?raw=true)

![Derived](https://github.com/NiekBeijloos/SOLID/blob/master/3.%20Liskov%20Substitution%20Principle/Examples/History%20rule/Solution/Derived.png?raw=true)

Now our **Initalize** function will set the state of the LifeCycle and the Valve will keep track of its own state. We completly shifted responsibilities and resolved the LSP violation.
