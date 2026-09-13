This project was an attempt at understanding sockets and how to transmit data from one source to another. 

The main idea was to have a server, which hosted a simulation of a 2D grid, and clients, which are capable of interacting 
with the grid. The network is done through TCP. 

The build was done on Mac and you would have to do some changes in the CMake file etc. to make 
the sockets compatible with windows in particular. 

The main structure of this project can be found in modules. This contains the following:
graphics
input
network
physics

Graphics: 
The graphics module controls how the grid is drawn on client windows. It contains both the setup of the actual windows 
through GLFW as well as the object, which draws on this window using OpenGL. The shader module contains how the fragment shader and
the vertex shader is setup. As this is just drawing a grid it is straightforward.

Input:
The input module explains how input on the client windows are handled. This is again using GLFW and looks at how you are 
drawing your mouse, when holding left-click. The packet struct explains the information send from client to server.

Network:
The network module contains two different objects, the Server class and the Client class. 
The Server can listen out for incoming clients, add them to its network and broadcast arrays to all the clients.
The Client can connect to a server, receive array data and send back packets which signal interactions on the grid. 

Physics:
The physics module contains the main simulation loop done on the server. How the grid is defined, how to update its rows
in a parallelizable manner etc. 

Finally, the src folder contains the main source for the client and server executables. 

