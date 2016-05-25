# skel

[![Build Status](https://travis-ci.org/Temelio/ansible-role-skel.svg?branch=master)](https://travis-ci.org/Temelio/ansible-role-skel)

Manage the skeleton structures used with account creation or another file
prerequisites.

## Requirements

This role requires Ansible 2.0 or higher,
and platform requirements are listed in the metadata file.

## Testing

This role contains two tests methods :
- locally using Vagrant
- automatically with Travis

### Testing dependencies
- install [Vagrant](https://www.vagrantup.com)
- install [Vagrant serverspec plugin](https://github.com/jvoorhis/vagrant-serverspec)
    $ vagrant plugin install vagrant-serverspec
- install ruby dependencies
    $ bundle install

### Running tests

#### Run playbook and test

- if Vagrant box not running
    $ vagrant up

- if Vagrant box running
    $ vagrant provision

## Role Variables

### Default role variables

    skel_default_owner: 'root'
    skel_default_group: 'root'
    skel_default_directory_mode: '0750'
    skel_default_file_mode: '0640'

    skel_default_link_force: False
    skel_default_link_mode: '0640'
    skel_default_link_state: 'link'

    skel_entries: []

## How manage skels

Skels are defined in **skel_entries** var, and follow this syntax:

    skel_entries:
      - directories:
          - path: '/etc/skel/.ssh'
            mode: '0700'
        to_remove:
          - path: '/etc/skel/.bashrc'
      - directories:
          - path: '/etc/skel/foo'
          - path: '/etc/skel/bar'
            mode: '0755'
        links:
          - src: '/etc/skel/foo'
            dest: '/etc/skel/bar/foolink'
        to_copy:
          - src: './files/foo.txt'
            dest: '/etc/skel/bar/'
        to_template:
          - src: './files/bar.j2'
            dest: '/etc/skel/bar/bar.txt'

All lv1 keys are optional, you can mix them as you want.

### to_remove key

Use it to remove files from structure. It's the first task. Only one arg:
* path *(mandatory)*: full element path

### directories key

It manage directories creation. You can use these sub keys:
* path *(mandatory)*: full directory path
* state *(optional)*: default is 'directory'
* owner *(optional)*: default is managed by *skel_default_owner* var
* group *(optional)*: default is managed by *skel_default_group* var
* mode *(optional)*: default is managed by *skel_default_directory_mode* var

### links key

It manage links creation. You can use these sub keys:
* src *(mandatory)*: full source path
* dest *(mandatory)*: full destination path
* state *(optional)*: default is managed by *skel_default_link_state* var
* owner *(optional)*: default is managed by *skel_default_owner* var
* group *(optional)*: default is managed by *skel_default_group* var
* mode *(optional)*: default is managed by *skel_default_link_mode* var
* force *(optional)*: default is managed by *skel_default_link_force* var

### to_copy key

It manage element to be copied. You can use these sub keys:
* src *(mandatory)*: full source path
* dest *(mandatory)*: full destination path
* owner *(optional)*: default is managed by *skel_default_owner* var
* group *(optional)*: default is managed by *skel_default_group* var
* mode *(optional)*: default is managed by *skel_default_file_mode* var

### to_template key

It manage templating. You can use these sub keys:
* src *(mandatory)*: full source path
* dest *(mandatory)*: full destination path
* owner *(optional)*: default is managed by *skel_default_owner* var
* group *(optional)*: default is managed by *skel_default_group* var
* mode *(optional)*: default is managed by *skel_default_file_mode* var

## Dependencies

None

## Example Playbook

    - hosts: servers
      roles:
         - { role: Temelio.skel }

## License

MIT

## Author Information

Alexandre Chaussier (for Temelio company)
- http://www.temelio.com
- alexandre.chaussier [at] temelio.com
