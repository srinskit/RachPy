# RachPy - A Python client for Rach
Note: Only python3 is supported. Use pip3 and python3 if necessary. 
## Install
```
pip install rach --user
```


## Usage
Sample code. Run using `python test_1.py`
```
# test_1.py

import time
from Rach import Rach

if __name__ == '__main__':
    rach = Rach("ws://localhost:8080", {'username': 'su', 'password': 'pass'})
    # Enable debug messages
    rach.enable_debug()
    rach.start()
    # Wait for connection
    time.sleep(1)
    # Subscribe
    one_two_sub = rach.add_sub('/one/two', lambda x: print(x), [])
    # Call a service
    rach.service_call('/remainder', [5, 2], lambda x, y: print(x, y), [6], lambda err, y: print(err, y), [3])
    rach.service_call('/client.public_id', [], lambda x: print(x), [], lambda err: print(err), [])
    try:
        while True:
            time.sleep(20)
    except KeyboardInterrupt:
        rach.stop()
        print('Bye!')
```
Sample code. Run using `python test_2.py`
```
# test_2.py

import time
from Rach import Rach

if __name__ == '__main__':
    rach = Rach("ws://localhost:8080", {'username': 'su', 'password': 'pass'})
    # Enable debug messages
    rach.enable_debug()
    rach.start()
    # Wait for connection
    time.sleep(1)
    # Register publisher
    one_two_pub = rach.add_pub('/one/two/')
    # Call a service
    rach.service_call('/version', [], lambda x: print(x.result), [], lambda err: print(err), [])
    rach.service_call('/client.public_id', [], lambda x: print(x.result), [], lambda err: print(err), [])
    try:
        while True:
            # Publish data
            one_two_pub.pub(time.time())
            time.sleep(5)
    except KeyboardInterrupt:
        rach.stop()
        print('Bye!')
```