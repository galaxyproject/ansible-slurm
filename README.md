Slurm
=====

Install and configure Slurm

Role Variables
--------------

All variables are optional. If nothing is set, the role will install the Slurm client programs, munge, and create a `slurm.conf` with a single `localhost` node and `debug` partition. See the [defaults](defaults/main.yml) and [example playbook](#example-playbook) for examples.

For the various roles a slurm node can play, you can either set group names, or add values to a list, `slurm_roles`.

- group slurmservers or `slurm_roles: ['controller']`
- group slurmexechosts or `slurm_roles: ['exec']`
- group slurmdbdservers or `slurm_roles: ['dbd']`

General config options for slurm.conf go in `slurm_config`, a hash. Keys are slurm config option names.

Partitions and nodes go in `slurm_partitions` and `slurm_nodes`, lists of hashes. The only required key in the hash is
`name`, which becomes the `PartitionName` or `NodeName` for that line. All other keys/values are placed on to the line
of that partition or node.

Set `slurm_upgrade` true to upgrade.

You can use `slurm_user` (a hash) and `slurm_create_user` (a bool) to pre-create a Slurm user (so that uids match). See 

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- name: Slurm all in One
  hosts: all
  vars:
    slurm_roles: ['controller', 'exec', 'dbd']
  roles:
    - galaxyproject.slurm
```

License
-------

MIT

Author Information
------------------

- [Nate Coraor](https://github.com/natefoo)
- [Helena Rasche](https://github.com/erasche)

[View contributors on GitHub](https://github.com/galaxyproject/ansible-slurm/graphs/contributors)
