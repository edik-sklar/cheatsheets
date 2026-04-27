## Classes and OOP

**What:** Blueprint for creating objects with shared attributes and methods.

**Basic class:**
class Server:
    def __init__(self, name, time):  # constructor — runs on creation
        self.name = name             # instance attribute
        self.time = time

    def __str__(self):               # what print() shows
        return f"{self.name} {self.time}"

    def is_slow(self):               # instance method — always takes self
        return self.time > 200

**Create and use:**
s = Server("web-01", 500)  # no .new() — just call the class
s.is_slow()                # True
print(s)                   # calls __str__

**Inheritance:**
class WebServer(Server):
    def __init__(self, name, time, port):
        super().__init__(name, time)  # call parent constructor
        self.port = port              # add new attribute

w = WebServer("web-01", 500, 8080)
w.is_slow()  # inherited from Server — works automatically

**Key rules:**
- self is always first parameter — Python passes it automatically
- __init__ never returns anything
- super().__init__() must pass parent's required args
- Child inherits all parent methods automatically

**Dunder methods — common ones:**
__init__    # constructor
__str__     # print() and str()
__len__     # len()
__eq__      # == comparison
__repr__    # developer representation

**SRE use case:**
class Alert:
    def __init__(self, server, severity, message):
        self.server = server
        self.severity = severity
        self.message = message

    def is_critical(self):
        return self.severity > 8

**Gotcha:**
self.name not name — always prefix with self inside methods
super().__init__() not super().__init__ — call it, don't just reference it