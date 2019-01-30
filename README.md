Slurm
=====

Install and configure Slurm

Role Variables
--------------

All variables are optional. If nothing is set, the role will install the Slurm client programs, munge, and create a `slurm.conf`. See the [defaults][defaults] and [example playbook](#example-playbook) for examples.

For the various roles a slurm node can play, you can either set group names, or add values to a list, `slurm_roles`.

- group slurmservers or `slurm_roles: ['controller']`
- group slurmexechosts or `slurm_roles: ['exec']`
- group slurmdbdservers or `slurm_roles: ['dbd']`

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- name: Slurm all in One
  hosts: all
  vars:
    slurm_user: {}
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
