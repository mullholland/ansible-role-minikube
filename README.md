# [minikube](#minikube)

|GitHub|GitLab|
|------|------|
|[![github](https://github.com/mullholland/ansible-role-minikube/workflows/Ansible%20Molecule/badge.svg)](https://github.com/mullholland/ansible-role-minikube/actions)|[![gitlab](https://gitlab.com/mullholland/ansible-role-minikube/badges/master/pipeline.svg)](https://gitlab.com/mullholland/ansible-role-minikube)|[![quality](https://img.shields.io/ansible/quality/unset)](https://galaxy.ansible.com/mullholland/minikube)|

description

## [Role Variables](#role-variables)

These variables are set in `defaults/main.yml`:
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


## [Example Playbook](#example-playbook)

This example is taken from `molecule/default/converge.yml` and is tested on each push, pull request and release.
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

The machine needs to be prepared in CI this is done using `molecule/default/prepare.yml`:
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
          - "iproute2"  # dependenciy minikube
          - "ethtool"  # dependenciy minikube
          - "socat"  # dependenciy minikube
        state: present
      when:
        - ansible_distribution in [ "RedHat", "CentOS", "Amazon", "Rocky", "AlmaLinux", "Fedora" ]

    - name: Install dependencies
      ansible.builtin.package:
        name:
          - "conntrack"  # Kubernetes 1.23.3 requires conntrack to be installed in root's path
          - "iproute2"  # dependenciy minikube
          - "ethtool"  # dependenciy minikube
          - "socat"  # dependenciy minikube
        state: present
      when:
        - ansible_os_family == "Debian"

# Possible driver for local installation
#   roles:
#     - role: mullholland.docker
```





## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/mullholland):

-   [debian9](https://hub.docker.com/r/mullholland/docker-molecule-debian9)
-   [debian10](https://hub.docker.com/r/mullholland/docker-molecule-debian10)
-   [debian11](https://hub.docker.com/r/mullholland/docker-molecule-debian11)
-   [ubuntu1804](https://hub.docker.com/r/mullholland/docker-molecule-ubuntu1804)
-   [ubuntu2004](https://hub.docker.com/r/mullholland/docker-molecule-ubuntu2004)
-   [centos7](https://hub.docker.com/r/mullholland/docker-molecule-centos7)
-   [centos-stream8](https://hub.docker.com/r/mullholland/docker-molecule-centos-stream8)
-   [centos-stream9](https://hub.docker.com/r/mullholland/docker-molecule-centos-stream9)
-   [ubi8](https://hub.docker.com/r/mullholland/docker-molecule-ubi8)
-   [fedora34](https://hub.docker.com/r/mullholland/docker-molecule-fedora34)
-   [fedora35](https://hub.docker.com/r/mullholland/docker-molecule-fedora35)
-   [amazonlinux](https://hub.docker.com/r/mullholland/docker-molecule-amazonlinux)
-   [rockylinux8](https://hub.docker.com/r/mullholland/docker-molecule-rockylinux8)
-   [almalinux8](https://hub.docker.com/r/mullholland/docker-molecule-almalinux8)

The minimum version of Ansible required is 2.10, tests have been done to:

-   The previous versions.
-   The current version.
-   The [devel](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-devel-from-github-with-pip) version.





If you find issues, please register them in [GitHub](https://github.com/mullholland/ansible-role-minikube/issues)

## [License](#license)

MIT


## [Author Information](#author-information)

[Mullholland](https://github.com/mullholland)

## [Special Thanks](#special-thanks)

Template inspired by [Robert de Bock](https://github.com/robertdebock)
