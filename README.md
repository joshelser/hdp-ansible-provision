# Bootstrap'ing nodes for fun and profit

## First time only

### Install Ansible
You need to install Ansible to use it. On OSX, this is easy:
```
$ brew install ansible
```

### Setup SSH
Make sure you have password-less SSH set up. Use the appropriate key here. Update `base.yml` to have the same key.
```
$ cat ~/.ssh/config
Host *.your.tld
  User root
  IdentityFile ~/.ssh/your-super-secret-key.pem
  StrictHostKeyChecking no
```

## Using Ansible

Create a "hosts" file with your nodes. We are creating one in the default location, tagging all hosts with the group
"hdp".
```
$ cat /etc/ansible/hosts                                                                                                                                                                                   -
[hdp]
host1.your.tld
host2.your.tld
host3.your.tld
host4.your.tld
host5.your.tld
host6.your.tld

[ambari-server]
host1.your.tld
```

Set up the basic node. `-f` controls the level of parallelism (set this to the number of nodes you're interacting with
to make this go faster).
```
$ ansible-playbook base.yml -f 6
```

Then, set up Ambari on these nodes:
```
$ ansible-playbook hdp.yml -f 6
```

Finally, take the path to your blueprint (cluster.json and blueprint.json files), and have Ambari install that blueprint.
Make sure your cluster.json is updated for the right hostnames! (blueprints not included because reasons)
```
$ ./load-blueprint.sh ~/.ssh/your-super-secret-key.pem ~/blueprints/hdp3.0.0/6-node <ambari-server>
```
