# [Ansible role minikube](#minikube)

Installs and configures minikube

|GitHub|Downloads|Version|
|------|---------|-------|
|[![github](https://github.com/mullholland/ansible-role-minikube/actions/workflows/molecule.yml/badge.svg)](https://github.com/mullholland/ansible-role-minikube/actions/workflows/molecule.yml)|[![downloads](https://img.shields.io/ansible/role/d/mullholland/minikube)](https://galaxy.ansible.com/mullholland/minikube)|[![Version](https://img.shields.io/github/release/mullholland/ansible-role-minikube.svg)](https://github.com/mullholland/ansible-role-minikube/releases/)|
## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/mullholland/ansible-role-minikube/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
  become: true
  gather_facts: true

  # pre_tasks:
  #   # create local user
  #   - name: Create system group
  #     ansible.builtin.group:
  #       name: "minikube"
  #       state: present

  #   - name: Create system user
  #     ansible.builtin.user:
  #       name: "minikube"
  #       group: "minikube"
  #       shell: "/bin/bash"
  #       state: present
  roles:
    - role: "mullholland.minikube"

    # tasks:
    #   # start minikube
    #   - name: start minikube
    #     ansible.builtin.command:
    #       cmd: "minikube start"
    #       creates: nothing
    #     become: yes
    #     become_user: "minikube"
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/mullholland/ansible-role-minikube/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  become: true
  gather_facts: true

  tasks:
    - name: Install dependencies
      ansible.builtin.package:
        name:
          - "conntrack"  # Kubernetes 1.23.3 requires conntrack to be installed in root's path
          - "iproute"  # minikube dependency
          - "ethtool"  # minikube dependency
          - "socat"  # minikube dependency
        state: present
      when:
        - ansible_distribution in [ "RedHat", "CentOS", "Amazon", "Rocky", "AlmaLinux", "Fedora" ]

    - name: Install dependencies
      ansible.builtin.package:
        name:
          - "conntrack"  # Kubernetes 1.23.3 requires conntrack to be installed in root's path
          - "iproute2"  # minikube dependency
          - "ethtool"  # minikube dependency
          - "socat"  # minikube dependency
        state: present
      when:
        - ansible_os_family == "Debian"

# Possible driver for local installation
#   roles:
#     - role: mullholland.docker
```



## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/mullholland/ansible-role-minikube/blob/master/defaults/main.yml):

```yaml
---
# Install minikube for local usage.
minikube_version: "v1.25.2"
minikube_os: "linux"
minikube_arch: "amd64"

minikube_mirror: "https://github.com/kubernetes/minikube/releases/download"
minikube_install_dir: "/usr/bin"

# `minikube start` should start as a non-root-user.
# This should be an exising user on the Linux system.
# https://minikube.sigs.k8s.io/docs/start/
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/mullholland/ansible-role-minikube/blob/master/requirements.txt).


## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://mullholland.net) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/mullholland/ansible-role-minikube/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/mullholland):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/r/mullholland/enterpriselinux)|all|
|[Amazon](https://hub.docker.com/r/mullholland/amazonlinux)|Candidate|
|[Fedora](https://hub.docker.com/r/mullholland/fedora/)|all|
|[Ubuntu](https://hub.docker.com/r/mullholland/ubuntu)|all|
|[Debian](https://hub.docker.com/r/mullholland/debian)|all|

The minimum version of Ansible required is 2.10, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/mullholland/ansible-role-minikube/issues).

## [License](#license)

[MIT](https://github.com/mullholland/ansible-role-minikube/blob/master/LICENSE).

## [Author Information](#author-information)

[Mullholland](https://mullholland.net)
