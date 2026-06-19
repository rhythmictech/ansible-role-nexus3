Ansible Role: Sonatype Nexus 3
==============================

Installs and configures [Sonatype Nexus Repository OSS 3](https://www.sonatype.com/products/nexus-repository)
on RedHat-family Linux, optionally fronted by an Apache (httpd) reverse proxy.

Requirements
------------

- Ansible 2.10 or higher.
- A RedHat-family distribution: RHEL / CentOS / Rocky / Alma 7–9, or Amazon Linux 2 / 2023.
- When `nexus_use_apache` is enabled, an already-installed and managed Apache (httpd).
  This role only drops the vhost config into `/etc/httpd/conf.d/` and reloads httpd; it
  does not install Apache itself.

Role Variables
--------------

| Variable | Default | Description |
|----------|---------|-------------|
| `nexus_user` | `nexus` | Service account user. |
| `nexus_group` | `{{ nexus_user }}` | Service account group. |
| `nexus_uid` | _(unset)_ | Pin the user's UID. Leave unset to let the OS allocate one. |
| `nexus_gid` | _(unset)_ | Pin the group's GID. Leave unset to let the OS allocate one. |
| `nexus_version` | `3.21.2-03` | Nexus release to install. |
| `nexus_download_base_url` | `https://download.sonatype.com/nexus/3/` | Base URL for the release tarball. |
| `nexus_base_dir` | `/opt/nexus` | Install root. Contains `nexus-<version>/`, `current` symlink, and `sonatype-work/`. |
| `nexus_application_host` | `127.0.0.1` with Apache, else `0.0.0.0` | Address Nexus binds to. |
| `nexus_application_port` | `8081` | Port Nexus binds to. |
| `nexus_base_url` | _(unset)_ | `ServerName` used in the Apache vhost. |
| `nexus_context_path` | `/` | Web context path. |
| `nexus_heap_size` | `1200M` | JVM `-Xms`/`-Xmx`. |
| `nexus_maxdirectmem` | `2G` | JVM `-XX:MaxDirectMemorySize`. |
| `nexus_use_apache` | `true` | Front Nexus with an Apache reverse proxy. Set `false` to expose Nexus directly. |
| `nexus_docker_registries` | `[]` | Docker registry reverse-proxy vhosts (only used when `nexus_use_apache`). Each item: `name`, `apache_port`, `nexus_port`. |

When `nexus_use_apache` is `false`, no httpd config is written and `nexus_application_host`
defaults to `0.0.0.0` so Nexus is reachable directly.

Example Playbook
----------------

```yaml
- hosts: nexus
  become: true
  roles:
    - role: ansible-role-nexus3
      vars:
        nexus_version: 3.21.2-03
        nexus_uid: 3000
        nexus_gid: 3000
        nexus_use_apache: false
```

License
-------

MIT
