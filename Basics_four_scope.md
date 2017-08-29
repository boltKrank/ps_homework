#Basics Four: Scope

## 'include' vs 'require'

  ```
  class simClass{
    require tom
    require harry
    ...
  }
  ```

  In the above example, using require means that the classes 'tom' and 'harry' need to be applied before any of the resources in the 'simClass' class. So you would use this if any resources in 'simClass' are dependant on the existence of anything in the 'tom' or 'harry' class.

## Parameter variables:
```
class simon(
  $variable1 = 'tom',
  $variable2 = 'harry',
  ){  
}

define defrec(
  $variable3 = 'bob',
  $variable4 = 'karl',
  ){


  }

```

The above examples shows parameter variables for defined resources as well as for classes. Placing the variable assignments within parenthesis after the class or defined variable name makes the variables parameter variables. This makes the values available throughout that class. In templates you can access these variables using classname::variable_name or just the variable name on its own for ERB templates.


## Resource defaults:

```
parentclass{

  File {
    ensure => file,
    mode   => '0644',
  }

  include childclass
}
```

The above resource default means that all resources of type file will have a mode of 644 and will be a file unless overriden. Any file resources in the class 'childclass' will inherit the parameters from this resource default unless it contains its own File resource default. Also, if the File resource default of 'childclass' didn't contain a 'mode' parameter - it will inherit this parameter from the 'parentclass's resource default.

## Inheritence in defined resources:

Like resource defaults being inherited in included classes, defined resources also inherit the resource defaults from the calling class.
However, the 'inherits' keyword used in classes can't be used in defined resources.



## Resource collectors for overriding resource values:

Even though resources are declared with set values, the values contained in resource collectors override the values of any resources they refer to.

Example:

```
file { '/tmp/collector.txt':
  ensure => file,
  content => 'This is the from the resource declaration'
}

File<||>{
  content => 'This is from the collector'
}

```

In the above situation, even though we've set the file's contents in the file resource declaration, it'll be overrided by the collector. So in this case, the contents of the file /tmp/collector.txt will be the text: 'This is from the collector'




## Issues arising from using inheritance:

Inheritance can make it easier to get values from other areas, but when unexpected values arise - tracing the source of the variable's value can be hard. Since Puppet relies on modulepaths instead of OO-style packages, class inheritance isn't strict and can refer to any class in the namespace, whereas a similar duplicate inheritance would raise issues instantly in most IDEs.
Also - with Puppet's philosophy in building functionality from classes then modules - have variable values inherited from a multitude of different classes goes against common functionality grouping methods used in Puppet. Multiple inheritance also makes refactoring of code laborious as it unnecessarily couples values to multiple implementations.

## Resource defaults in site.pp

Resource defaults used in site.pp within node definitions are inherited by all classes included in the node definition (similar to as described above re: classes). If it is done outside the node definitions, then it is inherited by all nodes within that environment.

## Puppet node classifier:

- SCOPE: variables in the node classifier are passed in as parameters for the class and as a result are treated as class variables (can be inherited from if classes are included within that class, or if used in a resource default).

- OVERRIDING: Variables passed in at the node classifier can't be overridden. If they are not passed in Puppet will use Hiera lookup and then result to the default if available. Otherwise it will be an undefined variable.

- CHECKS AND ADVANTAGES: The advantage of things being input via the console is that they are easy to look up and see via classification groups. Also, you can query these results via the PuppetDB API. At the moment, if data-types are specified, then Puppet will make sure the inputted value isn't blank, and matches such a type, or in some cases such as String - converting the inputted value to that type.
