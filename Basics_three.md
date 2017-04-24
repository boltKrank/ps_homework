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

  NOTE: uid is automatically generated.

### Create a local group

  Creating a group called "team apj":
  ```
  group{"team apj":
    ensure => present,
  }
  ```

  NOTE: gid is automatically generated.

### Add the user to the group



### Grant your user the "Log on as a Service" right
#### Explain what the "Log on as a Service" right does
## File and directory permissions
### Create a new directory
#### Set its owner to the user you created above using the owner attribute
#### Set its group to the group you created above using the group attribute
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
