# Basics Two: MCollective/ActiveMQ.

## mCollective connections / data flow:

*How do messages get to their subscribers ?*
Via the mco command and the middleware defined in the configuration, messages get sent to the broker, which then sends the message out to the nodes that match the grouping/filter. Once the MCollective agents receive the message they perform any commands or actions needed.

*What is the typical data flow ?*
Client  (where the mco command is run)   ->  Middleware (ActiveMQ or RabbitMQ) -> MCollective agent -> runs actions


## mCollective subcollectives:

A “collective” is a logical grouping of nodes/MCollective servers, a “subcollective” is the same in that it’s a grouping, but it also has a parent collective that it’s a member of, so it can be referenced specifically or via its parent collective


## mCollective publishers and servers:

- Security ?

## mCollective - multiple brokers:

*Can MCollective be configured to connect to more than one ActiveMQ Broker?*
Yes. You can have a cluster of ActiveMQ Brokers connected to an ActiveMQ hub.
In the client settings (`/etc/mcollective/server.cfg`)

```
plugin.activemq.pool.size = x
```

Where `x` is the number of brokers you want.


## mCollective - multiple brokers in parallell:



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






#### Requirements:

Focus on determining how ActiveMQ and MCollective work.
Environment requires at least one PE master and one PE agent (separate machines)
Use network analysis tools (tcpdump)
Determine who makes the connection; the ActiveMQ broker or the MCollective service?
How do messages get to the subscribers?
Data flow
What are subcollectives? How are they implemented?
What allows a MCollective publisher to interact with a MCollective server? Think security measures.
Can MCollective be configured to connect to more than one ActiveMQ Broker?
Can MCollective connect to more than one ActiveMQ Broker at one time?
Can an ActiveMQ Hub be configured to allow or deny certain subcollectives to/from certain ActiveMQ Spokes......how would you do this?
What is the different between a MCollective Agent and a MCollective Application?
How do you fault find MCollective issues? Where are the logs? How do you change the log levels?
How are MCollective agents synced?
If you performed `mco ping` and got 20 returns, why would some other mco subcommand only attempt to communicate with three nodes?
Think about network security devices, such as firewalls, in the environment. How do you change how often MCollective sends a keepalive packet?
How do you place security on individual actions?  Can you lock down users and their abilities?
Write a report on how ActiveMQ and MCollective work, include in the report answers to all the above points
