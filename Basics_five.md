# Basics Five: Workflow including r10k and eyaml

### Environments:


#### Development:

**PuppetFile:**
```

```

**environment.conf:**
```
```
**site.pp:**
```
```


#### Staging:
**PuppetFile:**
```

```


#### Production:
**PuppetFile:**
```

```


### Modules:

**Git version (init.pp):**

```
class gitversion{

  

}
```


Investigate how r10k works in an environment.
PE puppetserver
PE puppet agent (separate server)
GIT server (separate)
Build an all-in-one Puppet master that should be configured with r10k and have at least the following environments:
Production
Development
Staging
Each environment should have its own hiera directory structure and use yaml and eyaml (https://github.com/TomPoulton/hiera-eyaml).
The environments must have the following modules (add more as required):
Production (current default module versions for PE):
puppetlabs/stldib
puppetlabs/concat
puppetlabs/apache
puppetlabs/mysql
Development:
puppetlabs/stldib (4.2.2 or a newer/different version to Production)
puppetlabs/concat (1.1.0 or a newer/different version to Production)
puppetlabs/apache
puppetlabs/mysql
Staging:
puppetlabs/stldib (4.1.0 or a newer/different version to Production)
puppetlabs/concat (1.1.0 or a newer/different version to Production)
puppetlabs/apache (default)
puppetlabs/mysql (default)
You need to create two modules:
1. Module that creates a fact to return the current version of git installed.
2. Module to manage Apache and MySQL with 5 website and 5 databases.
Both modules must be in GIT and have three or more branches.
Roles and Profiles must be used.
The Profiles must use Hiera, and Automatic Data Binding should be disabled (via Puppet)
You must be able to show that a change in the git code changes the environments and a new branch in GIT will make a new branch in Puppet (for the PuppetFile for r10k).
Write a report on the above, including your code and how you installed/configured r10k and eyaml.
