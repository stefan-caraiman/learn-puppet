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