# [Ansible role ansible-generator](#ansible-generator)

Installs and configures minikube

|GitHub|Downloads|Version|
|------|---------|-------|
|[![github](https://github.com/mullholland/ansible-role-ansible-generator/actions/workflows/molecule.yml/badge.svg)](https://github.com/mullholland/ansible-role-ansible-generator/actions/workflows/molecule.yml)|[![downloads](https://img.shields.io/ansible/role/d/mullholland/ansible-generator)](https://galaxy.ansible.com/mullholland/ansible-generator)|[![Version](https://img.shields.io/github/release/mullholland/ansible-role-ansible-generator.svg)](https://github.com/mullholland/ansible-role-ansible-generator/releases/)|
## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/mullholland/ansible-role-ansible-generator/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all
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
    - role: "{{ lookup('env', 'MOLECULE_PROJECT_DIRECTORY') }}"

    # tasks:
    #   # start minikube
    #   - name: start minikube
    #     ansible.builtin.command:
    #       cmd: "minikube start"
    #       creates: nothing
    #     become: yes
    #     become_user: "minikube"
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/mullholland/ansible-role-ansible-generator/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
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

The default values for the variables are set in [`defaults/main.yml`](https://github.com/mullholland/ansible-role-ansible-generator/blob/master/defaults/main.yml):

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

- pip packages listed in [requirements.txt](https://github.com/mullholland/ansible-role-ansible-generator/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[mullholland.docker](https://galaxy.ansible.com/mullholland/docker)|[![Build Status GitHub](https://github.com/mullholland/ansible-role-docker/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-docker/actions)|[![Build Status GitLab](https://gitlab.com/mullholland-github-mirror/ansible-role-docker/badges/master/pipeline.svg)](https://gitlab.com/mullholland-github-mirror/ansible-role-docker)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://mullholland.net) for further information.

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/mullholland):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/r/mullholland/enterpriselinux)|all|
|[Rocky](https://hub.docker.com/r/mullholland/rockylinux)|all|
|[AlmaLinux](https://hub.docker.com/r/mullholland/almalinux)|all|
|[Amazon](https://hub.docker.com/r/mullholland/amazonlinux)|all|
|[Fedora](https://hub.docker.com/r/mullholland/fedora/)|all|
|[Ubuntu](https://hub.docker.com/r/mullholland/ubuntu)|all|
|[Debian](https://hub.docker.com/r/mullholland/debian)|all|
|[CentOS](https://hub.docker.com/r/mullholland/centos)|all|

The minimum version of Ansible required is 2.10, tests have been done to:

- The version before the previous version.
- The previous version.
- The current version.

If you find issues, please register them in [GitHub](https://github.com/mullholland/ansible-role-ansible-generator/issues).

## [License](#license)

[MIT](https://github.com/mullholland/ansible-role-ansible-generator/blob/master/LICENSE).

## [Author Information](#author-information)

[Mullholland](https://mullholland.net)
