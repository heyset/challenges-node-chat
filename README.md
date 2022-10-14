# Node Chat Challenge

## Objective
Build a client/server system based in the Web Sockets Protocol using only Node native modules, that allows instant message exchange with fan-out, where clients connected to the same "room" receive all the messages sent by any of them.

## Rules
- You can't follow a step-by-step tutorial
- You can't use any library (package), **except** libraries for automated tests, if you want to add them
- You *can* consult:
  - Node official docs
  - Stack overflow
  - Ask for help in specific parts
  
## Requirements
- You should have two javascript files, at least:
  - server.js (or .mjs)
  - client.js (or .mjs)
- You can include more files if you wish to
- Expected behavior from each file:
  - server.js, when ran with `node server.js`:
    - The server will open in any port of your choosing, and begins accepting connections in this port
    - The connections are always following the Web Socket protocol and must include a "room"
    - If the room doesn't exist, it must be created
    - If the room exists, the client is added to the list of clients in that room
    - Any message received in a room is delivered to all clients connected to the same room
    - Clients connected to a room don't receive messages from other rooms
    - The server generates a unique ID for each client, and is capable of sending messages by itself both for individual clients and for the whole room, without necessarily needing a client to send a message
    - When a client connects to a room, it is necessary to show all the message history in that room to that client, and it's needed to communicate all clients in that room that a new client has connected
    - Consider that the room will only last while the server is up (which means, you don't have to persist the messages if the server goes down), but the room must remain open even if there is no client connected, since it can receive new connections
    - All the messages received from any client and every time a client connects is displayed in the server's stdout (probably the terminal)
  - client.js, when ran with `node client.js <room-name>`, considering the server is up
    - Connects to the server in the room `<room-name>` and keeps the terminal open
    - Any message typed in the terminal (through `stdin`) is sent to the server
    - Any message received from the server is shown in the terminal (`stdout`)
- You must have a solution so that a client doesn't see the message it just sent twice.
  - This solution can be server-side, client-side, or both
  
## Tips
- You'll probably want to use `http` module
- You may want to use the `crypto` module to generate unique IDs
- It's usually easier to create the server first
- Keep your code clean and organized, since it will be easier for you to reason about it

## Bonus requirements
(not in any particular order)
(PS: any of those only really count if all the basic requirements are already finished)
- Automated tests
- The server persists the history even when it is shut down
  - If you want to persist in a DB, feel free to install that DB's module
  - Or you can persist in local disk, using `fs` module
    - Bonus bonus requirement
      - The messages are stored in an "append-only" log, open in stream mode when the server is up
      - When a message comes in, it is appended to the log specific to that room
      - Whenever a client logs in, the server reads the full history and sends it to the client by streaming (not keeping the buffer in memory)
- Clients can choose a nickname
- The client that opens a new room can define a password, and any new clients that connect must inform that password to connect
