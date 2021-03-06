From "Cracking the Coding Interview" book solution.

### Solution

By applying the word synchronized to a method, we ensure that two threads cannot execute synchronized methods on the same object instance at the same time.

So, the answer to the first part really depends. If the two threads have the same instance of the object, then no, they cannot simultaneously execute method A. However, if they have different instances of the object, then they can.

Conceptually, you can see this by considering locks. A synchronized method applies a "lock" on all synchronized methods in that instance of the object. This blocks other threads from executing synchronized methods within that instance.

In the second part, we're asked if threadl can execute synchronized method A while thread2 is executing non-synchronized method B. Since B is not synchronized, there is nothing to block threadl from executing A while thread2 is executing B. This is true regardless of whether threadl and thread2 have the same instance of the object.

Ultimately, the key concept to remember is that only one synchronized method can be in execution per instance of that object. Other threads can execute non-synchronized methods on that instance, or they can execute any method on a different instance of the object.
