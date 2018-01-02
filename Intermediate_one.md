# Intermediate One: Scaling and HA

## Plan:

1. Create Puppet master of Masters
2. Create load balancer
3. Create compile master
4. Create node to connect to load balancer -> compile master -> MoM and test.
5. Create a 2nd compile master, change node to connect to this via the load balancer and test.
6. Make the compile masters AMQ Hubs
7. Setup compile masters to join proxy automatically (no human interaction)


### 1. Create master of masters

Simple install complete, hostname is mom1.localdomain

### 2. Create load loadbalancer
```
class profile::loadbalancer {

  class { 'haproxy' :
    global_options => {
      user  => 'root',
      group => 'root',
    },
  }

  haproxy::listen { "loadbalancer":
    collect_exported => false,
    ipaddress        => $::ipaddress,
    ports            => '8140',
  }

  haproxy::balancermember { 'compile1':
    listening_service => 'loadbalancer',
    server_names      => 'compile1.localdomain',
    ipaddresses       => '192.168.0.42',
    ports             => '8140',
    options           => 'check',
  }

}
```

### 3. Create compile masters

Hostnames: compile1.localdomain and compile2.localdomain.

SSH into compile1 and run:

`curl -k https://mom1.localdomain:8140/packages/current/install.bash | sudo bash -s main:dns_alt_names='compile1.localdomain','compile1'`

SSH into mom1 and run:

`puppet cert --allow-dns-alt-names sign compile1`

Via PE Console, pin console1 to the PE infrastructure/PE Master group.

Add loadbalancer.localdomain to pe_repo::compile_master_pool_address

### 4. Create node for testing.

On a blank node, adding the following to the /etc/hosts file:
- 192.168.0.43 mom1.localdomain mom1 loadbalancer.localdomain loadbalancer

This allows the agent package to be served from the compile master, as opposed to directly from the MoM.

### 5. Add another compile master.

Same as before, plus adding the extra balancermember to the ha_proxy profile.

`curl -k https://mom1.localdomain:8140/packages/current/install.bash | sudo bash -s main:dns_alt_names='compile2.localdomain','compile2'`

### 6. Make the compile masters AMQ hubs:

https://puppet.com/docs/pe/2017.3/installing/installing_activemq_hubs_and_spokes.html

## Creating an HA replica:

To create an HA replica from a vanilla PE Master install:

-



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
