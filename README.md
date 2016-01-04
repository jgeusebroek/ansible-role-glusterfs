# Ansible role: glusterfs

An Ansible Role that installs and configures GlusterFS Java on RedHat/CentOS or Debian/Ubuntu.

## Requirements

Epel repository when using RedHat / CentOS. You could use [ansible-role-epel](https://galaxy.ansible.com/detail#/role/6522) for this.

## Dependencies

None

## Example Playbook

    ---
    - hosts: all
      become: true
      vars:

      glusterfs_volumes:
          - name: www-data
            cluster: ['{{ ansible_fqdn }}']
            host: '{{ ansible_fqdn }}'
            options:
              nfs.disable: 'on'
              auth.allow: '10.10.20.*'
            force: True
            bricks:
              - /gluster-exports/www-data

      glusterfs_mounts:
          - mount_dir: '/var/www'
            volume: '{{ ansible_fqdn }}:www-data'
            owner: 'root' # optional
            group: 'root' # optional
            mode: '0755' # optional

      roles:
       - { role: jgeusebroek.glusterfs, tags: ["glusterfs"] }

## Example Variables

	# Use the 'official' glusterfs packages instead of the 'outdated' distro packages
	glusterfs_use_glusterfs_repo: True

	# When using the 'official' packages, which version to install.
	#
  # Example: LATEST, '3.7/3.7.6' or 3.7 (for Ubuntu)
  #
	# RedHat: http://download.gluster.org/pub/gluster/glusterfs/{{ glusterfs_repo_version }}/EPEL.repo/epel-$releasever/$basearch/
	# Debian: http://download.gluster.org/pub/gluster/glusterfs/{{ glusterfs_repo_version }}/Debian/{{ ansible_distribution_release }}/apt/
	# Ubuntu: ppa:gluster/glusterfs-{{ glusterfs_repo_version }}
	#
	glusterfs_repo_version: "{% if ansible_distribution == 'Ubuntu' %}3.7{% else %}LATEST{% endif %}"

	# By default, don't install the server package
	glusterfs_install_server: False

	# By default, install the client package
	glusterfs_install_client: True


Have a look at the playbook example or the source for the dict syntax. Mostly all [glustermodule parameters](http://docs.ansible.com/ansible/gluster_volume_module.html) are supported.

	# By default no glusterfs volumes will be created and mounted.
	glusterfs_volumes: {}
	glusterfs_mounts: {}

## License

MIT / BSD

## Author Information

This role was created in 2015 by [Jeroen Geusebroek](http://jeroengeusebroek.nl/).