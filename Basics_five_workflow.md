# PS Basics Five workflow

## Setting up Puppet Code Manager:

### Code repo:

The control repo used for this section is available at:

git@github.com:boltKrank/control-repo.git

At this point there is only 1 branch: 'production' (instead of the default master branch), but we'll be adding more branches after.

Once the key-pair has been generated on the master, the public key is uploaded to this repository to give the master access. And a copy of the private key is put in /etc/puppetlabs/puppetserver/ssh (then chown'd to pe-puppet).


### Puppet Master settings:

To point the master to this repository, the puppet_enterprise::profile::master class has the following parameters updated:
- code_manager_auto_configure => true
- r10k_remote => git@github.com:boltKrank/control-repo.git
- r10k_private_key => /etc/puppetlabs/puppetserver/ssh/id-control_repo.rsa

### Create a deployment role:

We'll need a user for the purpose of creating auth tokens for Code Manager to use, for this purpose we'll create a user called "deploy".
In the console, under "Access Control" -> "Users", we'll add a new user "deploy" adding that user to the "Code Deployers" role.

Once this user has been created and password has been set, we can request a token which will allow us to run the puppet-code command.

Token requested with the following syntax:
```
puppet-access login --service-url https://<HOSTNAME OF PUPPET ENTERPRISE CONSOLE>:4433/rbac-api
```

Entering the username and password for our "deploy" user.

Now that we have the token, we can run
```
puppet-code deploy --dry-run
```

And we get the following output:
```
--dry-run implies --wait.
--dry-run implies --all.
Dry-run deploying all environments.
Found 1 environments.
```

The 1 environment being the 'production' branch.

## Setting up our environments:

To mimic how this may look in the real world, we'll start with the 'production' environment, deliberately using some "older" versions of modules and code, and the branching in to the staging then development environments we can show newer version.

### Production environment:

*Puppetfile:*
forge "http://forge.puppetlabs.com"
mod "puppetlabs/stdlib", '4.17.1'
mod "puppetlabs/concat", '3.0.0'
mod "puppetlabs/apache", '1.10.0'
mod "puppetlabs/mysql", '3.9.0'




### Staging environment:

*Puppetfile:*
forge "http://forge.puppetlabs.com"
mod "puppetlabs/stdlib", '4.18.0'
mod "puppetlabs/concat", '4.0.0'
mod "puppetlabs/apache", '1.11.0'
mod "puppetlabs/mysql", '3.10.0'



### Development environment:

*Puppetfile:*
forge "http://forge.puppetlabs.com"
mod "puppetlabs/stdlib", '4.19.0'
mod "puppetlabs/concat", '4.0.1'
mod "puppetlabs/apache", '2.0.0'
mod "puppetlabs/mysql", '3.11.0'
