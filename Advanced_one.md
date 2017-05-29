Advanced One: Virtual Resources, Auto-tagging, and Scope
The PSE should use the module downloadable from https://github.com/beergeek/scoping.git
The PSE requires a full PE master and agent (a separate agent makes this easier).
Look at the module and understand what is being attempted.


###Classify the node with scoping::test_out, run Puppet and observe results.

*Output:*
```
Info: Using configured environment 'production'
Info: Retrieving pluginfacts
Info: Retrieving plugin
Info: Loading facts
Info: Caching catalog for agent.localdomain
Info: Applying configuration version 'bc7f8b3943cb8d43e741dc2af532ef071690d0fe'
Notice: /Stage[main]/Scoping::Base/Host[agent.localdomain]/ip: ip changed '172.17.0.3' to '192.168.62.194'
Notice: /Stage[main]/Scoping::Base/Host[agent.localdomain]/host_aliases: defined 'host_aliases' as 'ca'
Info: Computing checksum on file /etc/hosts
Notice: /Stage[main]/Scoping::Base/Host[h0.puppetlabs.vm]/ensure: created
Notice: /Stage[main]/Scoping::Base/Host[localhost]/ip: ip changed '::1' to '127.0.0.1'
Notice: /Stage[main]/Scoping::Base/Host[localhost]/host_aliases: host_aliases changed 'ip6-localhost ip6-loopback' to 'localhost.localdomain localhost4 localhost4.localdomain4'
Notice: /Stage[main]/Scoping::Base/Host[pdb.puppetlabs.vm]/ensure: created
Notice: /Stage[main]/Scoping::Base/Host[ip6-localnet]/ensure: removed
Notice: /Stage[main]/Scoping::Base/Host[ip6-mcastprefix]/ensure: removed
Notice: /Stage[main]/Scoping::Base/Host[ip6-allnodes]/ensure: removed
Notice: /Stage[main]/Scoping::Base/Host[ip6-allrouters]/ensure: removed
Notice: /Stage[main]/Scoping::Base/Host[pe-puppet.localdomain]/ensure: removed
Notice: Applied catalog in 2.10 seconds
```

###Reclassify the node with scoping::test_in_0, run Puppet and observe.
```
Warning: Unable to fetch my node definition, but the agent run will continue:
Warning: Connection refused - connect(2) for "pe-puppet.localdomain" port 8140
Info: Retrieving pluginfacts
Error: /File[/opt/puppetlabs/puppet/cache/facts.d]: Failed to generate additional resources using 'eval_generate': Connection refused - connect(2) for "pe-puppet.localdomain" port 8140
Error: /File[/opt/puppetlabs/puppet/cache/facts.d]: Could not evaluate: Could not retrieve file metadata for puppet:///pluginfacts: Connection refused - connect(2) for "pe-puppet.localdomain" port 8140
Info: Retrieving plugin
Error: /File[/opt/puppetlabs/puppet/cache/lib]: Failed to generate additional resources using 'eval_generate': Connection refused - connect(2) for "pe-puppet.localdomain" port 8140
Error: /File[/opt/puppetlabs/puppet/cache/lib]: Could not evaluate: Could not retrieve file metadata for puppet:///plugins: Connection refused - connect(2) for "pe-puppet.localdomain" port 8140
Info: Loading facts
Error: Could not retrieve catalog from remote server: Connection refused - connect(2) for "pe-puppet.localdomain" port 8140
Warning: Not using cache on failed catalog
Error: Could not retrieve catalog; skipping run
Error: Could not send report: Connection refused - connect(2) for "pe-puppet.localdomain" port 8140
```

###Explain, IN DETAIL, why all the virtual resources were realised.
###Reclassify the node with scoping::test_in_1 and prove your theory is correct.

*Output:*
```

```

Send your explanation to your manager.






##Analysis:

*test_out.pp:*

```
class scoping::test_out {
  include scoping::base
  scoping::outside { 'h0.puppetlabs.vm': }
  scoping::outside { 'pdb.puppetlabs.vm': }

}
```

*outside.pp:*
```
define scoping::outside (
) {

  Host <| tag == $name |>

}
```
