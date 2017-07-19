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

  define windows_user::logon_service (
    String  $user,
    String  $domain,
    Boolean $allow,
    ){

      user { $user:
        ensure => present,
      }

      if($allow){
        exec { "ntrights +r SeServiceLogonRight -u \"${domain}\${user}\"":
          require => User[$user],
        }   
      }
      else{
        exec { "ntrights -r SeServiceLogonRight -u \"${domain}\${user}\"":
          require => User[$user],
        }   
      }
  }

  windows_user::logon_service{ 'tom':
    user    => 'tom',
    domain  => 'home',
    allow   => true,
  }

  ```




  ntrights +r SeServiceLogonRight -u "Domain\Administrator"


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

*output:*
```
PS C:\ProgramData\PuppetLabs\code\environments\production\modules> puppet resource file "C:/tmp/exampledir"
file { 'C:/tmp/exampledir':
  ensure => 'directory',
  ctime  => '2017-04-24 03:10:16 +0000',
  group  => 'S-1-5-21-1953236517-242735908-2433092285-1002',
  mode   => '2000700',
  mtime  => '2017-04-24 03:10:16 +0000',
  owner  => 'S-1-5-21-1953236517-242735908-2433092285-1001',
  type   => 'directory',
}
```

Permissions based on mode is : 2000700


### Manage the directory permissions using the acl module



#### Grant the local user Full Control

  ```
  acl{ 'C:/tmp/exampledir':
    permissions => [
      {identity => 'tim', right => ['full']}
    ],
  }
  ```

#### Grant the local group Read Only permissions

  ```
  acl{ 'C:/tmp/exampledir':
    permissions => [
      {identity => 'team apj', rights => ['read']}
    ]
  }
  ```

#### How do Windows ACLs differ from unix filesystem permissions?


To add.
http://www.networkworld.com/article/2230977/microsoft-subnet/comparing-access-control-in-windows-and-linux.html


## Registry
### Use the registry module to do the following:

#### Enable Internet Explorer enhanced security configuration

  ```
  registry_value {'HKLM\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}':
      ensure => present,
      type   => dword,
      data   => "1",
  }
  ```


#### Enable the Windows Shutdown Event Tracker

```
registry_value { 'HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Reliability':
  ensure  => present,
  type    => 'REG_DWORD'
  data    => 0,  
}
```


### Explain the difference between the following keys HKEY_LOCAL_MACHINE and HKEY_CURRENT_USER:

 - HKEY_CURRENT_USER is only applicable to one user while HKEY_LOCAL_MACHINE is applicable to all
 - HKEY_LOCAL_MACHINE is always available while HKEY_CURRENT_USER for a specific user is only available when he logs-in
 - HKEY_LOCAL_MACHINE are loaded on start-up while HKEY_CURRENT_USER are loaded on log-in


## IIS

### Use the DISM, Windowsfeature or DSC (via Puppet) to install IIS

After installing the module:

```
puppet module install puppet-windowsfeature
```

####Windowsfeature:

```
  windowsfeature { 'Web-Server':
    ensure => present,
  }
```

####DSC:

```
  dsc_windowsfeature {'IIS':
    dsc_ensure => 'present',
    dsc_name   => 'Web-Server',
  }
```

### Use the opentable/iis module to create a basic website

```
  iis_site{ 'Basic Web Site':
    ensure => 'started',
    path   => 'C:\www\',
    port   => '8080',
  }

```
*NOTE: 8080 was used to avoid conflicts with the default site installed by IIS*
*Other option is to delete the default website via IIS Manager*

### Explain the difference between installing IIS and installing apache

Apache is installed as a standalone application running atomically from the windows features, whereas
IIS is installed as part of the windows feature set.


## Package Management

Using package resource:

```
  package{'7zip':
    ensure => installed,
    source => 'http://www.7-zip.org/a/7z1604-x64.msi',
  }
```

### Install 7-zip using the staging module

```
class zipinstaller {

  class {'staging':
    path => 'C:/tmp',
  }

  staging::file {'7z1700-x64.msi':
    source => 'http://www.7-zip.org/a/7z1700-x64.msi',
  }

  package{ '7zip':
    source  => "${staging::path}/zipinstaller/7z1700-x64.msi",
    require => Staging::File['7z1700-x64.msi'],
  }

}
```

### Install 7-zip using chocolatey

```
  package{'7zip':
    ensure    => latest,
    provider  => 'chocolatey',
  }
```


### Packages in Windows (and how they differ from Linux):

If using chocolatey as a provider, very little difference from a puppet point of view.
If not, it'll default to msi/exe which means a source installer needs to be specified. ensure => latest doesn't work as versioning isn't supported, and if the name of the package resource doesn't match the displayName Puppet will try to reinstall the package every time.

## Reboots


### Extend your 7-zip code to reboot the system after installing 7-zip

```
  package{'7zip':
    ensure    => latest,
    provider  => 'chocolatey',
  }

  reboot {'after':
    subscribe => Package['7zip'],
  }

```

#### Uninstall 7-zip and verify that the system reboots after reinstalling

Worked

#### Define the different conditions that can be used to trigger a reboot with the puppetlabs/reboot module. How do they differ from one another?

- Subscribe / notify: reboots after a refresh is sent, received.
- Pending: will reboot if there is a pending reboot waiting
- apply => finished: will only reboot once ALL packages in the catalog have been processed.
