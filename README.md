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

Example Playbooks
-----------------

Minimal setup, all services on one node:

```yaml
- name: Slurm all in One
  hosts: all
  vars:
    slurm_roles: ['controller', 'exec', 'dbd']
  roles:
    - galaxyproject.slurm
```

More extensive example:

```yaml
- name: Slurm execution hosts
  hosts: all
  roles:
    - galaxyproject.slurm
  vars:
    slurm_cgroup_config:
      CgroupMountpoint: "/sys/fs/cgroup"
      CgroupAutomount: yes
      CgroupReleaseAgentDir: "/etc/slurm/cgroup"
      ConstrainCores: yes
      TaskAffinity: no
      ConstrainRAMSpace: yes
      ConstrainSwapSpace: no
      ConstrainDevices: no
      AllowedRamSpace: 100
      AllowedSwapSpace: 0
      MaxRAMPercent: 100
      MaxSwapPercent: 100
      MinRAMSpace: 30
    slurm_config:
      AccountingStorageType: "accounting_storage/none"
      ClusterName: cluster
      FastSchedule: 1
      GresTypes: gpu
      JobAcctGatherType: "jobacct_gather/none"
      MpiDefault: none
      ProctrackType: "proctrack/cgroup"
      ReturnToService: 1
      SchedulerType: "sched/backfill"
      SelectType: "select/cons_res"
      SelectTypeParameters: "CR_Core"
      SlurmctldHost: "slurmctl"
      SlurmctldLogFile: "/var/log/slurm/slurmctld.log"
      SlurmctldPidFile: "/var/run/slurmctld.pid"
      SlurmdLogFile: "/var/log/slurm/slurmd.log"
      SlurmdPidFile: "/var/run/slurmd.pid"
      SlurmdSpoolDir: "/var/spool/slurmd"
      StateSaveLocation: "/var/spool/slurmctld"
      SwitchType: "switch/none"
      TaskPlugin: "task/affinity,task/cgroup"
      TaskPluginParam: Sched
    slurm_create_user: yes
    slurm_gres_config:
      - File: /dev/nvidia[0-3]
        Name: gpu
        NodeName: gpu[01-10]
        Type: tesla
    slurm_munge_key: "../../../munge.key"
    slurm_nodes:
      - name: "gpu[01-10]"
        CoresPerSocket: 18
        Gres: "gpu:tesla:4"
        Sockets: 2
        ThreadsPerCore: 2
    slurm_partitions:
      - name: gpu
        Default: YES
        MaxTime: UNLIMITED
        Nodes: "gpu[01-10]"
    slurm_roles: ['exec']
    slurm_user:
      comment: "Slurm Workload Manager"
      gid: 888
      group: slurm
      home: "/var/lib/slurm"
      name: slurm
      shell: "/usr/sbin/nologin"
      uid: 888
```

License
-------

MIT

Author Information
------------------

- [Nate Coraor](https://github.com/natefoo)
- [Helena Rasche](https://github.com/erasche)

[View contributors on GitHub](https://github.com/galaxyproject/ansible-slurm/graphs/contributors)
