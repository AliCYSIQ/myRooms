[[Room]]

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
by this end thread and  multi-threading