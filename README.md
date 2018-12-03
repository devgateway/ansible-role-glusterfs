# devgateway.glusterfs

Configure a GlusterFS volume. This role intentionally doesn't touch firewall settings, see example
for configuration reference.

## Optional Variables

### `glfs_brick_root`

The directory where bricks will be stored in subdirectories. Must not be on a root partition, or
setup will fail.

Default: ``` /var/lib/glusterfs ```

### `glfs_create_unit`

Whether to create, enable, and start a Systemd mount for the volume.

Default: ``` true ```

### `glfs_mount_point`

Where the GlusterFS volume will be mounted. If `glfs_create_unit` is `true`, this is required.

### `glfs_mount_timeout`

Configures the time to wait for the mount command to finish. See systemd.mount(5).

Default: 10

### `glfs_package`

Name of GlusterFS server package.

Default:

* **RedHat**: ``` glusterfs-server ```
* **Debian**: ``` glusterfs-server ```


### `glfs_systemd_dir`

Systemd user configuration directory.

Default: ``` /etc/systemd/system ```

### `glfs_volume`

Name of the GlusterFS volume.

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
