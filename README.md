Role Name
=========

An Ansible role for setting up autofs. There are two lists/dicts that are used:
- One for remotely populated maps (`autofs_empty_maps`, a list which will simply create a 
file <auto.foo> with `+auto.foo`).
- One for more complex configuration (`autofs_indirect_maps`) which (unlike what the var name says)
can be used for direct and indirect maps. This will also add the path to auto.master.

The example below will create a simple auto.home, a direct mount (auto.nobackup), and an indirect 
mount (auto.catdata). `mounts` requires `name` and `url` (the latter can be " " if desired, as shown 
below) and may take `options` (optional, will add '-') as parameters.


Requirements
------------

Role Variables
--------------

| Variable		| Default		| Comments (type) |
| :---			| :---			| :---		  |
| `autofs_empty_maps` | [] | See above. |
| `autofs_indirect_maps` | [] | See example playbook below. |
| `autofs_default_nfs` | 4 | NFS default version to be used with autofs |
| `autofs_sysconf_options` | "" | OPTIONS in /etc/sysconfig/autofs |
| `autofs_create_dirs` | [] | Directories to create |


Dependencies
------------

Example Playbook
----------------

```Yaml
- hosts: foo
  roles:
    - role: oscpe262.autofs 
  vars:
    autofs_empty_maps:
      - "auto.home"
    autofs_indirect_maps:
      - name: auto.nobackup
        path: /nobackup
        options: "rw,intr,async,nfsvers=3,actime=0,tcp,browse"
        mounts:
          - name: "nfs1"
            url: nfs1.example.com:/export/nobackup
      - name: auto.bardata
        path: '/'
        options: "rw,grpid,intr,noquota"
        mounts:
          - name: "+auto.bardata"
            url: " "
```


License
-------

MIT

Author Information
------------------

Issues, feature requests, ideas, suggestions, etc. are appreciated and can be posted in the Issues section. Pull requests are also very welcome. Please create a topic branch for your proposed changes, it's the easiest way to merge back into the project.

- [Oscar Petersson](https://github.com/oscpe262/) (Maintainer)

