# ROLE dginhoux.git_repos



## DESCRIPTION

This ansible role is used to manage git on specified local folder.<br />
It can init (not as bare at this time) ; config options and remotes ; launch actions.<br />
<br />
Usefull if you use a "work machine" and you wan to deploy on somes others.<br />
I use it to add/commit/push all my work git folders on my local gitea and gitlab as backup.<br />
I also use it on particular nodes to deploy new certs files.<br />
<br />
This playbooks just manage git, it's not designed to run any others pre/post tasks.


## REQUIREMENTS

#### SUPPORTED PLATFORMS

This role require a supported platform.<br />
It will skip node with unsupported platform to avoid any compatibility problem.<br />
This behaviour can be bypassed by settings the following variable `asserts_bypass=True`.

| Platform | Versions |
|----------|----------|
| Debian | buster, bullseye |
| Fedora | 33, 34, 35, 36, 37, 38 |
| EL | 7, 8 |

#### ANSIBLE VERSION

Ansible >= 2.13

#### DEPENDENCIES

None.



## INSTALLATION

#### ANSIBLE GALAXY

```shell
ansible-galaxy install dginhoux.git_repos
```
#### GIT

```shell
git clone https://github.com/dginhoux/ansible_role.git_repos dginhoux.git_repos
```


## USAGE

#### EXAMPLE PLAYBOOK

```yaml
- hosts: all
  roles:
    - name: start role dginhoux.git_repos
      ansible.builtin.include_role:
        name: dginhoux.git_repos
```


## VARIABLES

#### DEFAULT VARIABLES

Defaults variables defined in `defaults/main.yml` : 

```yaml
git_repos_install_binary: true

git_repos:
  - name: ansible_role_dginhoux.sysctl
    path: "/data/roles-infra/dginhoux.sysctl"
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
        url: "https://XXXXXXXXXXXXXXXXXXXXX@git-system.ginhoux.net/ansible/dginhoux.sysctl.git"
        state: present
      - name: github
        url: "https://AAAAAAAAAAAAAAAAAAAAA@github.com/dginhoux/ansible_role.sysctl.git"
        state: present
    actions:
      - # { action: command, command: "rm -r --cached .", ignore_error: true }
      - { action: add, content: "." }
      - { action: commit, message: "{{ git_repos_commit_msg }}" }
      - { action: command, command: "tag v1.2.0", ignore_error: true }
      - { action: push, to: gitea master, extra_params: "--tags" }
      - { action: push, to: github master, extra_params: "--tags", ignore_error: true }
```

#### DEFAULT OS SPECIFIC VARIABLES

Those variables files are located in `vars/*.yml` are used to handle OS differences.<br />
One of theses is loaded dynamically during role runtime using the `include_vars` module and set OS specifics variable's.

`NOT USED BY THIS ROLE`


## AUTHOR

Dany GINHOUX - https://github.com/dginhoux



## LICENSE

MIT
