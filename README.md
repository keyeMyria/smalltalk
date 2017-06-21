# Small Talk

[Demo Page](https://seanperfecto.github.io/applandingpage/)

Splash screen              |  Home screen
:-------------------------:|:-------------------------:
![splash](http://res.cloudinary.com/nearbytes/image/upload/c_scale,h_500,q_100/v1497275168/Screen_Shot_2017-06-12_at_6.44.21_AM_okqvwf.png)  |  ![categories](http://res.cloudinary.com/nearbytes/image/upload/c_scale,h_500,q_100/v1497275169/Screen_Shot_2017-06-12_at_6.45.20_AM_zjucm0.png)

Small Talk is a mobile video chat application that allows users to connect to other users via common subjects. With our category filters, our goal at Small Talk is to allow strangers to bond over similar interests.

## Technologies Used

The backend API was built using Python's Django framework while the frontend was constructed using React Native. React Native is useful for building performant iOS and Android apps. Transition and loading animations were incorporated to offer a polished experience for our users.

Other technologies implemented include:

- `WebRTC`
- `websockets`

## Features

#### WebRTC

To establish a video and peer-to-peer connection, WebRTC was implemented on top of Django Channels.  WebRTC is a technology that allows for peer-to-peer teleconferencing without requiring the user to install plug-ins or any other third-party software.  It's made up of a number of APIs and protocols that operate together in order to make this possible.  Most notably, the RTCPeerConnection interface represents the connection itself - mapping the local computer to the remote peer.  This object provides methods to connect to a remote peer, maintain and monitor the connection, and close the connection once it's no longer needed.  The process of connecting peers initially takes place through an external signaling server, where WebSockets are used to exchange important information about each user's media and connectivity options.  Whereas the recipient of the connection is typically explicitly specified by the user before the connection is made, our application does this work on the backend.  

#### Django Channels

In order to implement WebRTC, it was necessary to set up a signaling server so that two
users could establish a peer to peer connection. This involved implementing Django Channels, which
handles WebSocket connections. Redis was used for the channel layer, and a Daphne interface server was used in order to support both HTTP and WebSocket requests. Additional single-threaded worker servers were also set up in order to speed up response time and prevent hanging.

Upon the initial handshake, users are dynamically placed into chatrooms with each other (with a maximum of two users per chatroom).  A "connection" model was created to store these chatrooms in the database to maintain a reference to each chat and each user in the chat (via foreign keys). Because only the initial request resembles HTTP, and the reference to current user lives in the cookies sent over HTTP, it was necessary to extract a reference to current user upon this initial handshake to associate it with the WebSocket reply channel. When a second user enters a chat, a message is sent to the group to begin Peer to Peer negotiations, after which the channel simply relays messages between the two users to mediate WebRTC negotiations until a connection is established. When a chatroom becomes empty, it is destroyed.


## Future Implementations

#### Friendships
The friendships feature would allow users to add each other as friends if
they would like to call each other for future chat sessions.
