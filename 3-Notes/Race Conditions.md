[[Room]] [[week1]]

**A race condition.** is a situation in computer programs where the timing of events influences the behaviour and outcome of the program. It typically happens when a variable gets accessed and modified by multiple threads. Due to a lack of proper lock mechanisms and synchronization between the different threads, an attacker might abuse the system and apply a discount multiple times or make money transactions beyond their balance.

***multi Threading***
======================
In this task, we will provide a quick overview of the following terms:

- Program
- Process
- Thread
- Multi-threading

**Program** : program is set of instruction that design to do something ,and lit look like this:
```python
# Import the Flask class from the flask module
from flask import Flask

# Create an instance of the Flask class representing the application
app = Flask(__name__)

# Define a route for the root URL ('/')
@app.route('/')
def hello_world():
    # This function will be executed when the root URL is accessed
    # It returns a string containing HTML code for a simple web page
    return '<html><head><title>Greeting</title></head><body><h1>Hello, World!</h1></body></html>'

# This checks if the script is being run directly (as the main program)
# and not being imported as a module
if __name__ == '__main__':
    # Run the Flask application
    # The host='0.0.0.0' allows the server to be accessible from any IP address
    # The port=8080 specifies the port number on which the server will listen
    app.run(host='0.0.0.0', port=8080)
```
(don't need to understand what in this block of code)
this is set of codes or instruction or `Program
now process,

**process** is the program but when it running , this is what we called process.
now,
**Thread** in very simple way it the number of how much process could running in one time (multi-threading) like if you have 5 things and every run take 5 second and you use one thread to complete those five thing it will take **25 second** and if you use multi-threading it will take less time(that  on how much thread you have) for example you can running 5 threads you will need only 5 s to end this
(by this end thread and  multi-threading)

Race Conditions : real world analogy

image there's bank have account have 100 $ and Two threads try to withdraw  in same time
- the first thread try to withdraw 35$ 
- the second thread try to withdraw 45$
here the first thread set the account to 65$
and the second one set it 55$ 
We cannot be 100% certain which thread will get to update the remaining balance first; however, let’s assume that it is Thread 1. Thread 1 will set the remaining balance to $55. Afterwards, Thread 2 might set the remaining balance to $65 if not appropriately handled. (Thread 2 calculated that $65 should remain in the account after the withdrawal because the balance was $100 when Thread 2 checked it.) In other words, the user made two withdrawals, but the account balance was deducted only for the second one because Thread 2 said so!

## Example Code

Consider the following Python code with two threads simulating a task completion by 10% increments.

```python
import threading

x = 0  # Shared variable

def increase_by_10():
    global x
    for i in range(1, 11):
        x += 1
        print(f"Thread {threading.current_thread().name}: {i}0% complete, x = {x}")

# Create two threads
thread1 = threading.Thread(target=increase_by_10, name="Thread-1")
thread2 = threading.Thread(target=increase_by_10, name="Thread-2")

# Start the threads
thread1.start()
thread2.start()

# Wait for both threads to finish
thread1.join()
thread2.join()

print("Both threads have finished completely.")
```

These two threads start together; they do nothing except print a value on the screen. Consequently, one would expect them to finish simultaneously, or at least the result to be consistent. However, in the program above, there is no guarantee which thread will finish first and how early it will be. Below is the first execution output:

Terminal

           `python t3_race_to_100.py ... Thread Thread-1: 40% complete, x = 10 Thread Thread-2: 70% complete, x = 11 Thread Thread-1: 50% complete, x = 12 Thread Thread-2: 80% complete, x = 13 Thread Thread-1: 60% complete, x = 14 Thread Thread-1: 70% complete, x = 16 Thread Thread-2: 90% complete, x = 15 Thread Thread-2: 100% complete, x = 17 Thread Thread-1: 80% complete, x = 18 Thread Thread-1: 90% complete, x = 19 Thread Thread-1: 100% complete, x = 20 Both threads have finished completely.`
        

Below is a second execution output:

Terminal

           `python t3_race_to_100.py  ... Thread Thread-1: 70% complete, x = 10 Thread Thread-2: 40% complete, x = 11 Thread Thread-1: 80% complete, x = 12 Thread Thread-2: 50% complete, x = 13 Thread Thread-1: 90% complete, x = 14 Thread Thread-2: 60% complete, x = 15 Thread Thread-1: 100% complete, x = 16 Thread Thread-2: 70% complete, x = 17 Thread Thread-2: 80% complete, x = 18 Thread Thread-2: 90% complete, x = 19 Thread Thread-2: 100% complete, x = 20 Both threads have finished completely.`
        

Running this program multiple times will lead to different results. In the first attempt, Thread-2 reached 100 first; however, in the second attempt, Thread-2 reached 100 second. We have no control over the output. If the security of our application relies on one thread finishing before the other, then we need to set mechanisms in place to ensure proper protection. Consider the following two examples to better understand the bugs’ gravity when we leave things to chance.

On the AttackBox, you can save the above Python code and run it multiple times to observe the outcome. For instance, if you saved it as `race.py`, you can run the script using the `python race.py` command.

## Causes

As we saw in the last program, two threads were changing the same variable. Whenever the thread was given CPU time, it rushed to increase `x` by 1. Consequently, these two threads were “racing” to increment the same variable. This program shows a straightforward example happening on a single host.

Generally speaking, a common cause of race conditions lies in shared resources. For example, when multiple threads concurrently access and modify the same shared data. Examples of shared data are a database record and an in-memory data structure. There are many subtle causes, but we will mention three common ones:

- **Parallel Execution**: Web servers may execute multiple requests in parallel to handle concurrent user interactions. If these requests access and modify shared resources or application states without proper synchronization, it can lead to race conditions and unexpected behaviour.
- **Database Operations**: Concurrent database operations, such as read-modify-write sequences, can introduce race conditions. For example, two users attempting to update the same record simultaneously may result in inconsistent data or conflicts. The solution lies in enforcing proper locking mechanisms and transaction isolation.
- **Third-Party Libraries and Services**: Nowadays, web applications often integrate with third-party libraries, APIs, and other services. If these external components are not designed to handle concurrent access properly, race conditions may occur when multiple requests or operations interact with them simultaneously.
	 