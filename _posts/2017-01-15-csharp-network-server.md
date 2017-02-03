---
layout: post
title:  "C# Network Server (Multithreaded)"
date:   2017-01-15
tags: [C#,Networking]
references: [
   "MSDN Socket Examples:https://msdn.microsoft.com/en-us/library/w89fhyex(v=vs.110).aspx",
   "Async Socket:https://www.youtube.com/watch?v=cG5q4XdYIUI",
   "C# Chatroom:https://www.youtube.com/watch?v=X66hFZG5p3A "
]

excerpt: "In this article, we will learn the basics of socket programming in .NET Framework using C#. Secondly, we will create a small application consisting of a server and a client which will communicate using TCP and UDP protocols.Inter-Process Communication i.e. the capability of two or more physically connected machines to exchange data, plays a very important role in enterprise software development."
---

In this article, we will learn the basics of socket programming in .NET Framework using C#. Secondly, we will create a small application consisting of a server and a client which will communicate using TCP and UDP protocols.

Inter-Process Communication i.e. the capability of two or more physically connected machines to exchange data, plays a very important role in enterprise software development. TCP/IP is the most common standard adopted for such communication. Under TCP/IP each machine is identified by a unique 4 byte integer referred to as its IP address (usually formatted as 192.168.0.101). For easy remembrance, this IP address is mostly bound to a user-friendly host name. The program below (showip.cs) uses the System.Net.Dns class to display the IP address of the machine whose name is passed in the first command-line argument. In the absence of command-line arguments, it displays the name and IP address of the local machine.  


~~~~~~
port: PORT
~~~~~~~

### Testing code..

{% highlight csharp lineos %}
public class Server
    {
        Socket _listenSocket;
        List<ClientConnection> _clients;

        /// <summary>
        /// Constructor
        /// </summary>
        public Server()
        {
            _clients = new List<ClientConnection>();

        }

        /// <summary>
        /// Initialise a socket and bind it to local IP and port.
        /// Then start a Listener thread.
        /// </summary>

        public void InitServer()
        {
            _listenSocket = new Socket(AddressFamily.InterNetwork, SocketType.Stream,
                          ProtocolType.Tcp);
            string ip = NetUtility.GetActiveIPv4();

            IPEndPoint iep = new IPEndPoint(IPAddress.Parse(ip), 4000);

            _listenSocket.Bind(iep);

            Thread listenerThread = new Thread(Listen); //Start a listener thread
            listenerThread.Start();
            
        }

        /// <summary>
        /// This is a thread proc that listens for new client connection
        /// </summary>
        private void Listen()
        {
            while(true)
            {
                _listenSocket.Listen(0);
                _clients.Add(new ClientConnection(_listenSocket.Accept())); // client connected

            }
        }
    }
{% endhighlight %}

In this article, we will learn the basics of socket programming in .NET Framework using C#. Secondly, we will create a small application consisting of a server and a client which will communicate using TCP and UDP protocols.Inter-Process Communication i.e. the capability of two or more physically connected machines to exchange data, plays a very important role in enterprise software development. TCP/IP is the most common standard adopted for such communication. Under TCP/IP each machine is identified by a unique 4 byte integer referred to as its IP address (usually formatted as 192.168.0.101).

{% highlight csharp  %}
using System;
using System.Net;
class ShowIP{
    public static void Main(string[] args){
        string name = (args.Length < 1) ? Dns.GetHostName() : args[0];
        try{
            IPAddress[] addrs = Dns.Resolve(name).AddressList;
            foreach(IPAddress addr in addrs) 
                Console.WriteLine("{0}/{1}",name,addr);
        }catch(Exception e){
            Console.WriteLine(e.Message);
        }
    }
}
{% endhighlight %}