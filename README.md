# Docker Ansible Role

Installs Docker

## Requirements

This role requires Ansible 2.0 or higher.

## Role Variables

### Defaults

    - name: docker_version
      desc: Target Docker version to install
      default: 18.09.1 

    - name: docker_edition
      desc: Docker software edition (ce | ee)
      default: ce

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
      value: 'docker-{{ docker_edition }}'

    - name: docker_cli_package 
      desc: Docker CLI package name
      value: 'docker-{{ docker_edition }}-cli'

    - name: docker_package_version 
      desc: Version of Docker package to install from Apt 
      value: '5:{{ docker_version }}~3-0~{{ distribution }}-{{ distribution_release }}'

    - name: docker_group
      desc: Name of the docker system group
      value: docker

    - name: docker_path
      desc: Path to docker binary installation
      value: 

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
        - {{ docker_package }}
        - {{ docker_cli_package }}
        - docker-engine
        - docker.io
        - containerd
        - runc

    - name: docker_requisite_packages
      desc: Pre-requisite packages to install before Docker
      value:
        - apt-transport-https
        - ca-certificates
        - software-properties-common
        - curl

    - name: docker_packages
      desc: Docker package targets for installation (Must be simultaneously installed)
      value:
        - {{ docker_package }}={{ docker_package_version }}
        - {{ docker_cli_package }}={{ docker_package_version }}

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