# Basics Two: MCollective/ActiveMQ.

## mCollective connections / data flow:

*How do messages get to their subscribers ?*
Via the mco command and the middleware defined in the configuration, messages get sent to the broker (usually the middleware), which then sends the message out to the nodes that match the grouping/filter/subcollective. Once the mCollective agents receive the message they perform any commands or actions needed, for example: reply to ping, execute a command

*What is the typical data flow ?*
- Client  (where the mco command is run)   ->  Middleware (ActiveMQ or RabbitMQ) -> MCollective agent -> runs actions
- Output (if produced ) -> middleware -> client  (on the way back)


## mCollective subcollectives:

A “collective” is a logical grouping of nodes/MCollective servers, a “subcollective” is the same in that it’s a grouping, but it also has a parent collective that it’s a member of, so it can be referenced specifically or via its parent collective.
Doing it this way saves having to broadcast to every node when you only want/need to broadcast to a specific subset.


## mCollective publishers and servers:

*What allows a MCollective publisher to interact with a MCollective server? Think security measures.*

When a new node joins, it exchanges certificates with the middleware server, this is what sets up the secure link between node and middleware.

## mCollective - multiple brokers:

*Can MCollective be configured to connect to more than one ActiveMQ Broker?*
Yes. You can have a cluster of ActiveMQ Brokers connected to an ActiveMQ hub.
In the client settings (`/etc/mcollective/server.cfg`)

```
plugin.activemq.pool.size = x
```

Where `x` is the number of brokers you want.

*Can mCollective connect to more than one ActiveMQ Broker at one time ?*
mCollective has the option to connect to multiple brokers in a pool (as described above), but not at the same time.



## mCollective logging:

The log settings are as follows:

```
logger_type = console
loglevel = warn
logfile = /var/log/mcollective.log
keeplogs = 5
max_log_size = 2097152
logfacility = user
```

This is typically set in :  `~/.mcollective` or `/etc/mcollective/client.cfg`


## mCollective security:

*How do you change how often mCollective sends a keepalive packet ?*

`Registerinterval` setting can change the keepalive packet rate.

or in the STOMP1.1 heart-beat header:

```
CONNECT
heart-beat:<cx>,<cy>

CONNECTED:
heart-beat:<sx>,<sy>
```

If <cx> is 0 (the client cannot send heart-beats) or <sy> is 0 (the server does not want to receive heart-beats) then there will be none.
Otherwise, there will be heart-beats every MAX(<cx>,<sy>) milliseconds


*Can an ActiveMQ Hub be configured to allow or deny certain subcollectives to/from certain ActiveMQ Spokes......how would you do this?*

Using the authorizationPlugin, you can restrict the ability to read, write or administer (read+write) based on username and/or user group.


## Further questions:

*What is the different between a MCollective Agent and a MCollective Application?*

An agent, written in SimpleRPC communicates with the middleware receiving messages, acting on those messages and replying (when applicable).
An application works via the mco command and can be called via the command line to do certain things like list facts etc.

In usage - you would go through the middleware to communicate with an agent, and via the CLI to access an application.

*How are mCollective agents synced?*

Via comminication with the middleware.

*If you performed `mco ping` and got 20 returns, why would some other mco subcommand only attempt to communicate with three nodes?*

If you specify a specific subcollection or apply a filter, this will limit which nodes act upon the broadcast.
Certain nodes also - may not have permissions set to allow reading or writing via mco which will also reduce the replies coming back.

#### Requirements:


- [x] Determine who makes the connection; the ActiveMQ broker or the MCollective service?
- [x] How do messages get to the subscribers?
- [x] What are subcollectives? How are they implemented?
- [x] What allows a MCollective publisher to interact with a MCollective server? Think security measures.
- [x] Can MCollective be configured to connect to more than one ActiveMQ Broker?
- [x] Can MCollective connect to more than one ActiveMQ Broker at one time?
- [x] Can an ActiveMQ Hub be configured to allow or deny certain subcollectives to/from certain ActiveMQ Spokes......how would you do this?
- [x] What is the different between a MCollective Agent and a MCollective Application?
- [x] How do you fault find MCollective issues? Where are the logs? How do you change the log levels?
- [x] How are MCollective agents synced?
- [x] If you performed `mco ping` and got 20 returns, why would some other mco subcommand only attempt to communicate with three nodes?
- [x] Think about network security devices, such as firewalls, in the environment. How do you change how often MCollective sends a keepalive packet?
- [x] How do you place security on individual actions?  Can you lock down users and their abilities?
