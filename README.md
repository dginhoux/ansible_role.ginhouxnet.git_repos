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
  - name: ansible_role_dginhoux.git_repos
    path: "/ansible/roles/dginhoux.git_repos"
    state: present
    make_init: true
    make_conf_options: true
    make_conf_remotes: true
    make_actions: true
    ignores: ""
    configs:
      - { state: present, name: user.name, value: git }
      - { state: present, name: user.email, value: git@ginhoux.net }
      - { state: present, name: core.compression, value: 9 }
      - { state: present, name: core.sharedrepository, value: all }
      - { state: present, name: core.editor, value: vim }
      - { state: present, name: color.ui, value: "true" }
      - { state: present, name: format.pretty, value: oneline }
      - { state: absent, name: core.autocrlf, value: input }
      - { state: absent, name: core.logallrefupdates, value: "true" }
    remotes:
      - name: gitea
        url: "https://AZERTY@git-system.ginhoux.net/ansible/dginhoux.git_repos.git"
        state: present
      - name: github
        url: "https://AZERTY@github.com/dginhoux/ansible_role.git_repos.git"
        state: present
    actions:
      - { action: command, command: "rm -r --cached .", ignore_error: true, extra_params: "" }
      - { action: add, content: "." }
      - { action: commit, message: "{{ git_repos_commit_msg }}", extra_params: "--allow-empty" }
      - { action: push, to: gitea master, extra_params: "--force" }
      - { action: push, to: github master, ignore_error: true }
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
