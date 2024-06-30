This is used for real-time communication, to get updates we can use long/short polling (see more in [[System Design]]), requests done by a client. These do not allow us to get real time data as it is generated as it depends on the client to request at its own time. WS are <span style="color:#e1db3d">full-duplex</span> over a single persistent connection.
<span style="color:#e1db3d">Websockets</span> use a persistent TCP connection for all the server side updates.
<span style="color:#e1db3d">WebRTC</span> uses UDP where some data can be missed(not reliable).

<span style="color:#e1db3d">sockets.io </span>is a WS library, provides abstraction over the normal WS api, has a builtin feature of rooms, good for creating chat apps, not used in production.
WebHook 
