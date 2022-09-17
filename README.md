Ansible Role : dginhoux.git_repos
=========

This ansible role is used to manage git on specified local folder.
It can init (not as bare at this time) ; config options and remotes ; launch actions.

Usefull if you use a "work machine" and you wan to deploy on somes others.
I use it to add/commit/push all my work git folders on my local gitea and gitlab as backup.
I also use it on particular nodes to deploy new certs files

This playbooks just manage git, it's not designed to run any others pre/post tasks.


Requirements
------------

This role require a supported platform defined in `meta/main.yml`.
It will skip node with unsupported platform ; this behaviour can be bypassed by settings this variable `asserts_bypass=True`.


Role Variables
--------------



```yaml
git_repos_install_binary: true


git_repos:
  - name: ansible
    path: /ansible
    state: present
    make_init: true
    make_conf_options: true
    make_conf_remotes: true
    make_actions: true
    ignores: |
      **-data
      **-data?
      **-logs
    configs:
      - name: user.name
        value: git
        state: present
      - name: user.email
        value: git@ginhoux.net
        state: present
      - name: core.compression
        value: 0
        state: present
      - name: core.sharedrepository
        value: all
        state: present
    remotes:
      - name: origin
        url: https://xxxxxxxxxxxxxxxxxx@git.ginhoux.net/ansible.git
        state: present
    actions:
      - action: add
        conetnt: "."
      - action: commit
        message: "ok"
      - action: push
        to: origin master
      - action: pull
        from: origin master
      - action: command
        command: "fetch --all"

```


Dependencies
------------

none


Example Playbook
----------------



License
-------

BSD


Author Information
------------------

https://github.com/dginhoux/
