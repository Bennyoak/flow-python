# Python Flow Module

## Description

flow-python is a module to interact with the Flow stack using python.

## Install
```
$ git clone https://github.com/SpiderOak/flow-python.git
$ cd flow-python
$ sudo python setup.py install
```
## Basic Usage

Here's a simple script to list a user's Organizations/Teams:
```python
#!/usr/bin/env python
from flow import Flow

# Create flow intance and start using the API
flow = Flow('your-flow-username')

# Print user's organizations
print(flow.enumerate_orgs())

# Your application must make sure to call terminate when done with the flow object
flow.terminate()
```

Here's a script that listens for messages and prints them to stdout:
```python
#!/usr/bin/env python
from flow import Flow

flow = Flow('your-flow-username')

def print_message(data):
    regular_messages = data["RegularMessages"]
    for message in regular_messages:
        print("Got message '%s' from ChannelID='%s'" %
              (message["Text"], message["ChannelID"]))

# Here we register our callback to be executed when we receive a message
flow.register_callback(Flow.MESSAGE_NOTIFICATION, print_message)

try:
    # Once you registered all your callbacks, all you have to do is loop.
    flow.process_notifications()
except:
    flow.terminate()
```

## Comments

- If you intend to use one of the examples above, 'your-flow-username' should already be logged-in in your device.
- The app is responsible for calling `Flow.terminate()` before it quits. This is needed because the Flow module starts a separate local server process (semaphor-backend) and a separate thread to listen for notifications, `Flow.terminate()` cleans everything up. (*)
- All Flow API requests timeout after 30 seconds, with the exception of `Flow.wait_for_notification()`.
- An application should use a single instance of `flow.Flow`.

## TODO

- Implement remaining Flow API methods (e.g. `enumerate_peer_verifications`, `create_device`, etc.).
- Document all arguments of the Flow API. 
- Document Flow dict objects that are returned on many of the methods.
- Unit Testing the flow module.
- It has support for multiple sessions but this hasn't been tested yet.
- (*) Find a way to save this pain from the user.
- Remove unnecessary args in Flow's `__init__()`.
- See other TODOs in source code.
