+++
date = "2017-01-19T15:40:13+07:00"
title = "pupil servce"
weight = "8"
+++

<div class="header-border-top"></div>
<div class="content-container">
  <div class="header-link">
    <a href="#pupil-service">
      <h2 id="pupil-service">Pupil Service</h2>
    </a>
  </div>
</div>

<div class="content-container">
  <div class="header-link">
    <a href="#service-about">
      <h3 id="service-about">About</h3>
    </a>
  </div>
</div>
<div class="header-border-bottom"></div>

<p align="center">
	<img src="images/icons/ps.png" width="20%">
</p>

Pupil Service is like Pupil Capture except it does not have a world video feed or GUI. It is intended to be used with VR and AR eye tracking setups. 

Pupil Service is designed to run in the background and to be controlled via network commands only. The service process has no GUI. The tools introduced in the [hmd-eyes project](https://github.com/pupil-labs/hmd-eyes) are made to work with Pupil Service and Pupil Capture alike.

<div class="content-container">
  <div class="header-link">
    <a href="#talk-service">
      <h3 id="talk-service">Talking to Pupil Service</h3>
    </a>
  </div>
</div>
<div class="header-border-bottom"></div>

Code examples below demonstrate how to control Pupil Service over the network. 

> Starting and stopping Pupil Service:

```python
import zmq, msgpack, time
ctx = zmq.Context()

#create a zmq REQ socket to talk to Pupil Service/Capture
req = ctx.socket(zmq.REQ)
req.connect('tcp://localhost:50020')

#convenience functions
def send_recv_notification(n):
    # REQ REP requirese lock step communication with multipart msg (topic,msgpack_encoded dict)
    req.send_multipart(('notify.%s'%n['subject'], msgpack.dumps(n)))
    return req.recv()

def get_pupil_timestamp():
    req.send('t') #see Pupil Remote Plugin for details
    return float(req.recv())

# set start eye windows
n = {'subject':'eye_process.should_start.0','eye_id':0, 'args':{}}
print send_recv_notification(n)
n = {'subject':'eye_process.should_start.1','eye_id':1, 'args':{}}
print send_recv_notification(n)
time.sleep(2)


# set calibration method to hmd calibration
n = {'subject':'start_plugin','name':'HMD_Calibration', 'args':{}}
print send_recv_notification(n)


time.sleep(2)
# set calibration method to hmd calibration
n = {'subject':'service_process.should_stop'}
print send_recv_notification(n)
```

<div class="content-container">
  <div class="header-link">
    <a href="#notification">
      <h3 id="notification">Notifications</h3>
    </a>
  </div>
</div>
<div class="header-border-bottom"></div>

The code demonstrates how you can listen to all notification from Pupil Service. This requires a little helper script called [zmq_tools.py](https://github.com/pupil-labs/hmd-eyes/blob/master/hmd_calibration/zmq_tools.py).


```python
from zmq_tools import *

ctx = zmq.Context()
requester = ctx.socket(zmq.REQ)
requester.connect('tcp://localhost:50020') #change ip if using remote machine

requester.send('SUB_PORT')
ipc_sub_port = requester.recv()
monitor = Msg_Receiver(ctx,'tcp://localhost:%s'%ipc_sub_port,topics=('notify.',)) #change ip if using remote machine

while True:
    print(monitor.recv())
```

<div class="content-container">
  <div class="header-link">
    <a href="#clients">
      <h3 id="clients">Clients</h3>
    </a>
  </div>
</div>
<div class="header-border-bottom"></div>

An example client written in Python can be found [here](https://github.com/pupil-labs/hmd-eyes/tree/master/hmd_calibration)

An example client for Unity3d can be found [here](https://github.com/pupil-labs/hmd-eyes/tree/master/unity_integration_calibration)