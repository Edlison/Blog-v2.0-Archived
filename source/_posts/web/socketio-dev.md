---
title: Flask-SocketIO development experience
categories:
- Web
tags:
- python
- flask
---

# socketio init

socketio must input flask_app context, so the web service is constructed based socketio web service.



# join_room

If you want to create private room, you need to apply `join_room(room_id)` when users entering. After this, we can use `emit(to=room_id)` to send message to specific room.



# socketio.on

is used to receive message.



# socketio.emit

is used to send message.