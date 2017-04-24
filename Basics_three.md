# Basics Three: Windows
Familiarize yourself with common Windows administration tasks, and how they differ from their Linux counterparts:

## Local Accounts

### Create a local user

  Creating a user of name "tim":
  ```
  user{"tim":
    ensure => present,
  }
  ```

  *NOTE: uid is automatically generated.*

### Create a local group

  Creating a group called "team apj":
  ```
  group{"team apj":
    ensure => present,
  }
  ```

  *NOTE: gid is automatically generated.*

### Add the user to the group

  There are two ways of doing this:

  ```
  user{"tim":
    ensure => present,
    groups => ['team apj'],
  }
  ```

  *or*

  ```
  group{"team apj":
    ensure  => present,
    members => ['tim'],
  }
  ```

### Grant your user the "Log on as a Service" right

  ```
  user{"tim":
    ensure => present,
    groups => ['team apj'],

  }
  ```  


#### Explain what the "Log on as a Service" right does
*The Log on as a service user right allows accounts to start network services or services that run continuously on a computer, even when no one is logged on to the console.*



## File and directory permissions

### Create a new directory

```
  file{"C:/tmp/exampledir":
    ensure => directory,
  }

```

#### Set its owner to the user you created above using the owner attribute

```
  file{"C:/tmp/exampledir":
    ensure  => directory,
    owner   => 'tim',
  }

```
*NOTE: In the resource output the owner is recorded by the uid reference, not the actual username*


#### Set its group to the group you created above using the group attribute

```
  file{"C:/tmp/exampledir":
    ensure  => directory,
    owner   => 'tim',
    group   => 'team apj',
  }

```
*NOTE: In the resource output the group is recorded by the gid reference, not the actual group name*


#### Inspect the directory permissions



### Manage the directory permissions using the acl module
#### Grant the local user Full Control
#### Grant the local group Read Only permissions
#### How do Windows ACLs differ from unix filesystem permissions?
## Registry
### Use the registry module to do the following:
#### Enable Internet Explorer enhanced security configuration
#### Enable the Windows Shutdown Event Tracker
### Explain the difference between the following keys
#### HKEY_LOCAL_MACHINE
#### HKEY_CURRENT_USER
## IIS
### Use the DISM, Windowsfeature or DSC (via Puppet) to install IIS
### Use the opentable/iis module to create a basic website
### Explain the difference between installing IIS and installing apache
## Package Management
### Install 7-zip using the staging module
### Install 7-zip using chocolatey
## Reboots
### Extend your 7-zip code to reboot the system after installing 7-zip
#### Uninstall 7-zip and verify that the system reboots after reinstalling
#### Define the different conditions that can be used to trigger a reboot with the puppetlabs/reboot module. How do they differ from one another?
