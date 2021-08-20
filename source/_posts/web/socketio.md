---
title: Flask-Socktio
categories:
- Web
tags:
- python
- flask
- socket
---

# Start

```python
from flask_socketio import SocketIO

socketio = SocketIO(app)
```

# Receiving Messages

The event name is determined. `'message', 'json', 'xxx event'`, you can determine these name as socket name. Particularly `'connect', 'disconnecte'` can be called when connecting or disconnecting.

```python
@socketio.on('xxx event', namespace='/test')
def handler(json):
  print('receive json: ', str(json))
```

```html
<script>
	var socket = io.connect('http://0.0.0.0:5001')
  socket.on('connect', function() {
    socket.emit('xxx event', {'data': 'connected.'})
  })
</script>

```



# Sending Messages

`emit()` used for named events , `send()` used for unamed events.

```python
@socketio.on('xxx event', namespace='/chatroom')
def xxx_event():
  emit('xxx event', {'data': 'xxx'}, namespace='/chatroom')
```

**Important**

You can emit message in a router with `socketio.emit('xxx event', resp, namespace='/chatroom')`.

```python
@app.rout('/send')
def send():
	socketio.emit('xxx event', resp, namespace='/chatroom')
```

You can't use `emit()` without context of `socketio`, you should use `socketio.emit()`.



# Broadcasting

If broadcasting is enabled, all the clients connected to the namespace will receive the message.

```python
@socketio.on('xxx event', namespace='/chatroom')
def xxx_event():
  emit('xxx event', {'data': 'xxx'}, namespace='/chatroom', broadcast=True)
```



# Rooms

Users can receive messages from the room they are in.

```python
@socketio.on('join')
def on_join(data):
    username = data['username']
    room = data['room']
    join_room(room)
    send(username + ' has entered the room.', to=room)

@socketio.on('leave')
def on_leave(data):
    username = data['username']
    room = data['room']
    leave_room(room)
    send(username + ' has left the room.', to=room)
```

Use `to` argument set which room the message should be sent to.



# Class-Based Events

```python
from flask_socketio import Namespace, emit

class MyCustomNamespace(Namespace):
    def on_connect(self):
        pass

    def on_disconnect(self):
        pass

    def on_my_event(self, data):
        emit('my_response', data)

socketio.on_namespace(MyCustomNamespace('/test'))  # Assign namespace
```

All the event is defined by the methods with `on_` prefix.







----

Reference

https://flask-socketio.readthedocs.io