# Scoober Frontend Challenge Server Side

![](https://img.shields.io/badge/Code-NodeJs-informational?style=flat&logo=Node.js&logoColor=white&color=2bbc8a)
![](https://img.shields.io/badge/Code-Typescript-informational?style=flat&logo=typescript&logoColor=white&color=2bbc8a)
![](https://img.shields.io/badge/Editor-VsCode-informational?style=flat&logo=visualstudio&logoColor=white&color=2bbc8a)
![](https://img.shields.io/badge/Code-JsonServer-informational?style=flat&logo=json&logoColor=white&color=2bbc8a)
![](https://img.shields.io/badge/Tools-Socket.io-informational?style=flat&logo=socket.io&logoColor=white&color=2bbc8a)
![](https://img.shields.io/badge/Tools-Docker.io-informational?style=flat&logo=Docker&logoColor=white&color=2bbc8a)
![](https://img.shields.io/badge/Package-Npm-informational?style=flat&logo=npm&logoColor=white&color=2bbc8a)


# Introduction
This is the server part of the challenge that can be optionally used by the candidate to complete the frontend technical challenge. It is using socket.io to serve the communication between clients. Clients can subscribe to different events to complete the requirements.
​
## Technical Challenge
​
### Goal
​
The Goal is to implement a game with two independent units – the players – communicating with each other using an API.
​
### Description
​
When a player starts, they incept a random (whole) number and send it to the second player as an approach of starting the game. The receiving player can then choose between adding one of {-1,0,1} in order to get to a number that is divisible by 3. The resulting whole number is then sent back to the original sender.
​
The same rules are applied until one player reaches the number 1 (after the division. See example below.) https://www.figma.com/file/F914dSnSz01H1D1EY64ZXO/Game_of_three?node-id=0%3A1
​
Also, feel free to use the provided design screens and assets.

For each "move", a sufficient output should be generated (mandatory: the added, and the resulting number).
​
Players  should be also able to play individually  against the CPU 
​
### Notes & Requirements
​
- Each player has to be ​logged in before they start to play 
- Games should be playable under rooms, as default we are provided the rooms ( two rooms againts CPU and one ) please look at: **fakeDb/fake.json** or [API Docu](https://documenter.getpostman.com/view/17986533/UV5XhxLB)
- Each player runs on its own (independent programs, two browsers, web‐workers or a choice of your own).
- Communication via an API (REST, Sockets)
- A player may not be available when the other one starts
- Try to be platform independent, in other words the project must be runnable easily in every environment
- Please share your project on GitHub and send us the link
​
### Extras
​
Implementing a fancy UI using (and improving) provided design
[Design](https://www.figma.com/file/F914dSnSz01H1D1EY64ZXO/Game_of_three?node-id=0%3A1)

### **Required Technologies**
- React
- Redux
- type-checking libraries such as Typescript, Flow, Prop-Types
- Unit Tests (Jest etc...)

# Getting Started

First clone the repository in your local machine. You have two different options for starting the application: first is via **Docker** and the second is for **NON Docker Users**.

### **Docker Desktop Users**

 - Rename `.env.example` to `.env` 
 - Simply run `docker-compose up --build`

### **Non Docker Users**
In your root folder where the repository is cloned, run the following commands:
 - Rename `.env.example` to `.env` 
 - **CAUTION! please be sure that you start the npm commands inside `./wss/` folder**
 - `cd ./wss`
 - `npm install`
 - Starting the fake DB JsonServer: `npm run start:server` 
 - Open a new terminal and type: `npm run start`. This will establish the socket connection 


# Available Socket Events
Below you will find a list of available events, that might be useful during your development.

## Broadcast Events from Server => Client
 -  **`randomNumber`**
 -  **`activateYourTurn`**
 -  **`gameOver`**
 -  **`onReady`**
 -  **`error`**
 - **`message`**
 - **`listTrigger`**

## Fired events from Client Side => Server
- **`connection`**
- **`login`**
- **`joinRoom`**
- **`sendNumber`**
- **`leaveRoom`**
- **`letsPlay`**

### **connection**
When the connection is established via socket on client side.

### **login**
When the user is logged in the game, it creates a user data on fakeDB and broadcasts a `message` event after the promise is resolved.

### **joinRoom**
When the user join the room via client side.
It assings the client to the room, broadcasts a `message` event after the promise is resolved, and it lets the user join the room and broadcast an `onReady` event. If the room type is CPU, the `maxRoomSize` is 1. Otherwise, it will be waiting for the second opponent.

### **letsPlay**
After the event is fired from the client-side, it sends a request to fakeDb to get the user details. After success, it broadcasts the `randomNumber` event to the room, that will be visible for all opponents. Finally, it broadcasts `activateYourTurn` and the first connected user starts to play.

### **sendNumber**
It sends the number back to the client with the selected move choice ( 1,0,-1) and last calculated result of the number. It broadcasts the `randomNumber` event with the calculated result back and activates the opponent turn with the `activateYourTurn` event. For CPU users this section is automated, please look at the **code line 113**.

# Fake DB JsonServer
It's a server which creates a fake REST API. For more details, please check

[JSON Server Document](https://github.com/typicode/json-server)

By default we are set three rooms,
please check the `./fakeDb/fake.json` file

**Some Helpful API Methods**

[API DOCUMENTATION](https://documenter.getpostman.com/view/17986533/UV5XhxLB)
