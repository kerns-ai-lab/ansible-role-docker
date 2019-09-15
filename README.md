# Docker Ansible Role

Installs Docker

## Requirements

This role requires Ansible 2.0 or higher.

## Role Variables

### Defaults

    - name: docker_group_members
      desc: Users to add to the docker group.
      default: []

    - name: docker_compose_install
      desc: Flag determining whether to install docker-compose as part of installation
      default: True

    - name: docker_compose_version
      desc: Version of docker-compose to install
      default: 1.21.2

    - name: distribution
      desc: Operating system distribution
      default: ubuntu

    - name: distribution_release
      desc: Operating system distribution release
      default: xenial

### Vars

These are not meant to be modified

    - name: docker_package 
      desc: Docker package name [ce | ee]
      value: docker-ce

    - name: docker_group
      desc: Name of the docker system group
      value: docker

    - name: docker_repo_gpg
      desc: URL for GPG key to Docker repository
      value: "https://download.docker.com/linux/ubuntu/gpg

    - name: docker_repo_url
      desc: APT Repository URL for Docker
      value: "deb https://download.docker.com/linux/{{ distribution }} {{ distribution_release }} stable"

    - name: docker_remove_packages
      desc: Packages to be removed prior to Docker installation
      value:
        - docker
        - docker-ce
        - docker-engine
        - docker.io

    - name: docker_packages
      desc: Pre-requisite packages to install before Docker
      value:
        - apt-transport-https
        - ca-certificates
        - software-properties-common
        - curl

    - name: docker_compose_package
      desc: Name of docker-compose package
      value: docker-compose

    - name: docker_compose_path
      desc: Installation path of docker-compose
      value: /usr/local/bin/docker-compose

    - name: docker_compose_url
      desc: Target URL for downloading docker-compose
      value: "https://github.com/docker/compose/releases/download/{{ docker_compose_version }}/docker-compose-Linux-x86_64"


## Dependencies

None

## Example Playbook

Install Docker Vagrant

    - hosts: all
      vars:
        docker_group_members:
          - vagrant
      roles:
        - { role: ansible-role-docker }

Install Docker NUC

  	- hosts: all
  	  vars:
  	    docker_group_members:
          - docker_adman
          - user1
  	  roles:
  	   - { role: ansible-role-docker }

## Testing

Tests can be executed with local vagrantfile using the command. This mounts the role directory in the guest and executes the role via the playbook in `tests/` folder.

```shell
vagrant up
```