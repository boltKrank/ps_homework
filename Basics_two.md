# Basics Two: MCollective/ActiveMQ.
## Focus on determining how ActiveMQ and MCollective work.
## Environment requires at least one PE master and one PE agent (separate machines)
## Use network analysis tools (tcpdump)
## Determine who makes the connection; the ActiveMQ broker or the MCollective service?
## How do messages get to the subscribers?
## Data flow
## What are subcollectives? How are they implemented?
## What allows a MCollective publisher to interact with a MCollective server? Think security measures.
## Can MCollective be configured to connect to more than one ActiveMQ Broker?
## Can MCollective connect to more than one ActiveMQ Broker at one time?
## Can an ActiveMQ Hub be configured to allow or deny certain subcollectives to/from certain ActiveMQ Spokes......how would you do this?
## What is the different between a MCollective Agent and a MCollective Application?
## How do you fault find MCollective issues? Where are the logs? How do you change the log levels?
## How are MCollective agent plugins synced to their respective servers?
## If you performed `mco ping` and got 20 returns why would `mco r10k sync` only attempt to communicate with three nodes?
## Think about network security devices, such as firewalls, in the environment. How do you change how often MCollective sends a keepalive packet?
## How do you place security on individual actions?  Can you lock down users and their abilities?
## Write a report on how ActiveMQ and MCollective work, include in the report answers to all the above points
