# Intermediate One: Scaling and HA


In SLICE or VMPooler.

- Build a HA replica
- Build two compile masters that can use the primary and replica MoMs.
- Configure the MoMs as AMQ Hubs
- Configure the Compile Masters as AMQ brokers
- Compile Masters must join to the proxy without human interaction
- Ensure all non-Puppet-infrastructure node use the proxy/load balancer as their master and AMQ broker and NOT the MoM(s)
- Options for multiple CA servers in a multi master environment?
- Hooks or API triggering from the Git server or Build server for Code Manager.
- Policy-based auth-signing with CSR attributes.
- Certificate signing.
- DNS issues.
- How did you prevent the nodes connecting to the MoMs instead of the proxy?
- Write a report on how the above can be performed using Puppet where possible, including your code. Include the puppet.conf for a non-Puppet-infrastructure node, compile master and MoM.



## Report:
