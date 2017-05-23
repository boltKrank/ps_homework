# Basics Four: Scope

## Task requirements:

Get a deep understanding of Scope.

Think about things such as:

- Using variables with values from the parameter area of a class or defined resource (how you would do this?)
- Explain what influence a resource default in a calling class has on the included class.
- Can inheritance be used with defined resources?
- How can resource collectors be used to override resource values?  Provide examples with explanation.
- What issues can arise from using inheritance in Puppet to override variables and behaviour of parent class's resources?
- What occurs if you define a resource default in site.pp?
- Write a report on scope and include code examples.


## Report:

NOTE: For example's sake everything here will be done in a module called "myModule"

![Puppet Scope](/basics_four_images/scope.png)

**Top Scope:**

These variables are accessible everywhere, can be found in
- site.pp
- facts

**Node Scope:**

These are variables are set in the site.pp and are only accessible by nodes that are part of the group in which said variables are defined.
NOTE: Variables from the top scope (above) will override these variables.  s

**Local Scope:**

These are the variables defined within classes, defined types and lambdas. Accessible within the classes they're defined in as well as those classes' children.


**Static Scope:**

Classes can access local, node and top scope variables as well as a parent class's variables when `inherits` keyword is used.

**Dynamic Scope:**

Includes grandparents with the closest parent overriding its parent. i.e. when using a defined resource, the variables from the class where it is declared are passed down to it.

### Using params:

In a class:
```
class myClass(
    $param1 = "p1",
    $param2 = "p2",
  ){


}
```

or a defined resource:
```
define myClass::specialResouce(
    $magicNumber  = '80',
    recName       = $title,
  ){

}
```

Anything in the initial parenthesis can be passed through when the class or defined resource is declared.

i.e.:

### Resource defaults:

Resource defaults use dynamic scope.

### Resource collectors:
