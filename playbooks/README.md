# Usage

## Playbook 1: install or update certbot client

```shell
ansible-playbook -i inventory/hosts.ini -l windows playbooks/iinstall.yaml -vvvv
```

## Playbook 2: renew cert and install into windows

```shell
ansible-playbook -i inventory/hosts.ini -l windows playbooks/cert-renewal.yaml -vvvv
```

## Procedure to Prepare ansible box


`yum install ansible-core-2.13.5-1.el9`: This installs Ansible Core version 2.13.5-1 on a system running Red Hat Enterprise Linux (RHEL) version 9. This command uses the yum package manager to download and install the necessary packages and dependencies.

`yum install python3-pip`: This installs the pip package manager for Python 3 on a RHEL 9 system. Pip is used to install Python packages and libraries, including Ansible modules.

`python3 -m pip install --user --ignore-installed pywinrm`: This installs the pywinrm library for Python 3, which is used to communicate with Windows machines over WinRM (Windows Remote Management) protocol. The --user option installs the package for the current user only, and --ignore-installed option ignores previously installed versions of the package.

`ansible-galaxy collection install ansible.windows`: This installs the ansible.windows collection from Ansible Galaxy, which contains a set of Windows-specific modules and plugins for Ansible.

`ansible-galaxy collection install community.windows`: This installs the community.windows collection from Ansible Galaxy, which contains additional Windows modules and plugins contributed by the Ansible community.
