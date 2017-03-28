# Learning Puppet - Guide and how to based on the Learning Puppet VM

1. Commands for interacting with the "quests"

```bash
~ $ quest status
~ $ quest list
```
2. Puppet manifest structure and resources

```bash
type {'title':
    attribute => 'value',
}
```

- Getting resources through the Puppet RAL(Resource Abstraction Layer)

```bash
~ $ puppet resource user root

user { 'root':
  ensure           => present,
  comment          => 'root',
  gid              => '0',
  home             => '/root',
  password         => '$1$jrm5tnjw$h8JJ9mCZLmJvIxvDLjw1M/',
  password_max_age => '99999',
  password_min_age => '0',
  shell            => '/bin/bash',
  uid              => '0',
}

```

- List all the resource types
```
~ $ puppet resource --types
```
- Using puppet apply to execute Puppet code
```bash
~ $ puppet apply -e "user { 'argus': ensure => present, }"
```
- Using 'puppet resource' to modify/edit resources
```bash
~ $ puppet resource -e user argus
```
- This will open up a text editor, permitting us to modify the resource

**Note**
- '=>' is pronounced 'hash rocket'

3. Working with manifests and classes

```
~ $ cd /etc/puppetlabs/code/environments/production/modules
~ $ mkdir -p cowsayings/{manifests,examples}
~ $ vim cowsayings/manifests/cowsay.pp
```

- Now modify the puppet manifest so that it contains this class definition
```bash
class cowsayings::cowsay {
  package { 'cowsay':
    ensure   => present,
    provider => 'gem',
  }
}
```
- We can also validate the manifest using the 'puppet parser' tool
```
~ $ puppet parser validate cowsayings/manifests/cowsay.pp
```
- This is simply a class defintion, to declare it we would do the following:
```
~ $ vim cowsayings/examples/cowsay.pp
```

* In order to declare the cowsay class, we would use the 'include' keyword, add the following code snippet to the file: `include cowsayings::cowsay`
* Now to simply do a dry run, meaning that we would see only that changes that would be made on the system, we would add the --noop flag to the apply command

```
~ $ puppet apply --noop cowsayings/examples/cowsay.pp
# or to simply apply it, remove the flag
~ $ puppet apply cowsayings/examples/cowsay.pp
```

4. How to remove packages using `puppet resource`

```
~ $ puppet resource package cowsay ensure=absent provider=gem
```




---
**Notes:**
```
A class is a collection of related resources and other classes which, once defined, can be declared as a single unit. Puppet classes are also singleton, which means that unlike classes in object oriented programming, a Puppet class can only be declared a single time on a given node.

A manifest is a file containing Puppet code, and appended with the .pp extension. In this quest, we used manifests in the ./manifests directory each to define a single class, and used a corresponding test manifest in the ./examples directory to declare each of those classes.
```