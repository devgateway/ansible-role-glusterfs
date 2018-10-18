# devgateway.glusterfs

Role description.

## Required Variables

### `glfs_mount_point`

Variable description.

## Optional Variables

### `glfs_brick_root`

Variable description.

Default: ``` /var/lib/glusterfs ```

### `glfs_mount_timeout`

Variable description.

Default: 10

### `glfs_package`

Variable description.

Default:

* **RedHat**: ``` glusterfs-server ```
* **Debian**: ``` glusterfs-server ```


### `glfs_systemd_dir`

Variable description.

Default: ``` /etc/systemd/system ```

### `glfs_volume`

Variable description.

Default: ``` vol0 ```

## Playbook Example

    ---
    - hosts:
        - alpha
        - bravo
      tasks:
        - name: Install Firewalld
          package:
            name: firewalld

        - name: Start Firewalld
          service:
            name: firewalld
            state: started

        - name: Open GlusterFS ports
          firewalld:
            state: enabled
            permanent: true
            service: glusterfs

        - include_role:
            name: devgateway.glusterfs
          vars:
            glfs_mount_point: /opt


## License

GPLv3+

## Author Information

Copyright 2018, Development Gateway

