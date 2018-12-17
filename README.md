# devgateway.glusterfs

Configure a GlusterFS volume. This role intentionally doesn't touch firewall settings, see example
for configuration reference.

GlusterFS is picky about mountpoints:

1. A brick should be on a separate mountpoint.

2. A brick should not be in the file system root, but in a subdirectory.

Therefore, the role creates bricks at paths like `/srv/gluster/${glfs_volume}/brick`, so that you
can have a block device mounted at `/srv/gluster/${glfs_volume}`.

## Optional Variables

### `glfs_brick_root`

The directory where bricks will be stored in subdirectories.

Default: ``` /srv/gluster ```

### `glfs_brick_subdir`

The data subdirectory for each brick.

Default: ``` brick ```

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

### `glfs_replicas`

Number of replicas for the volume.

Default: `ansible_play_batch | length`

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
