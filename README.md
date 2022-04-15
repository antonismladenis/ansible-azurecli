Ansible Role Name
=========

azurecli

Description
------------

The [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/) is a set of commands used to create and manage Azure resources. The Azure CLI is available across Azure services and is designed to get you working quickly with Azure, with an emphasis on automation.

Purpose
------------

This role can be used in order to install (by default) or uninstall (if passed as variable) the Azure CLI to:

- Ubuntu 18.04 Linux machines
- Ubuntu 20.04 Linux machines
- RedHat 8.3 Linux machines

Requirements
------------

The Linux Debian target server requires the following packages:

- ca-certificates
- curl
- apt-transport-https
- lsb-release
- gnupg

Note: There is a task to install the prerequisites.

Role Variables
--------------

| Variable                               | Default Value                                                                                          | Description                                                                                                                                                                                                 |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| azurecli_prereqs_debian                | apt-transport-https, curl, ca-certificates, lsb-release, gnupg                                         | Package required by Azure CLI on Linux Debian based systems.                                                                                                                                                |
| azurecli_prereqs_desired_state_debian  | present                                                                                                | State of the azurecli_prereqs_desired_state_debian packages. Whether to install, verify if available or to uninstall (i.e. ansible apt module values: `present`, `latest`, or `absent`)                    |
| azurecli_repo_gpg_key_debian           | <https://packages.microsoft.com/keys/microsoft.asc>                                                    | Azure CLI GPG required on Debian based systems.                                                                                                                                                             |
| azurecli_repo_key_desired_state_debian | present                                                                                                | State of the `azure-cli` package. Whether to install, verify if available or to uninstall (i.e. ansible apt module values: `present`,`latest`, or`absent`)          |
| azurecli_repo_debian                   | deb [arch=amd64] <https://packages.microsoft.com/repos/azure-cli/> {{ansible_distribution_release}} main | Repository URL for Linux Debian based systems.                                                                                                                     |
| azurecli_repo_desired_state_debian     | present                                                                                                | `present` indicates creating the repository file if it doesn't exist on Linux Debian based systems. Alternative is `absent`(not recommended as it will prevent from installation of **azure-cli** pacakge). |
| azurecli_repo_baseurl_redhat           | <https://packages.microsoft.com/yumrepos/azure-cli>                                                    | Repository`baseurl`for Azure CLI on Linux RedHat based systems.                                                                                                                                             |
| azurecli_repo_gpg_key_redhat           | <https://packages.microsoft.com/keys/microsoft.asc>                                                    | Azure CLI GPG required on Linux RedHat based systems.                                                                                                                                                       |
| azurecli_repo_gpgcheck_redhat          | yes                                                                                                    | Boolean for whether to perform GPG check against Azure CLI on Linux RedHat based systems.                                                                                                                   |
| azurecli_repo_enabled_redhat           | yes                                                                                                    | Boolean for whether to set Azure CLI repo as 'enabled' on Linux RedHat based systems.                                                                                                                       |
| azurecli_repo_desired_state_redhat     | present                                                                                                | `present` indicates creating the repository file if it doesn't exist on Linux RedHAT based systems. Alternative is `absent`(not recommended as it will prevent from installation of **azure-cli** pacakge). |
| azurecli_desired_state                 | present                                                                                                | State of the `azure-cli` package. Whether to install, verify if available or to uninstall (i.e. ansible apt module values: `present`,`latest`, or`absent`)                                                  |

Dependencies
------------

No dependencies on other roles.

Example Playbook
----------------

For default behavior of role (i.e. installation of `azure-cli` package) in ansible playbooks.

```yaml
- hosts: servers
  roles:
    - azurecli
```

For customizing behavior of role (i.e. installation of latest `azure-cli` package) in ansible playbooks.

```yaml
- hosts: servers
  roles:
    - azurecli
  vars:
    azurecli_desired_state: latest
```

For customizing behavior of role (i.e. un-installation of `azure-cli` package) in ansible playbooks.

```yaml
- hosts: servers
  roles:
    - azurecli
  vars:
    azurecli_desired_state: absent
```

How to call the role
----------------

```bash
ansible-playbook -i inventory azurecli.yml
```

Note: Use azurecli_desired_state variable to specify if you want to install or uninstall Azure CLI.

Molecule Testing
----------------

[Molecule](https://molecule.readthedocs.io/en/latest/getting-started.html) is a complete testing framework that helps you develop and test Ansible roles, which allows you to focus on the content instead of focusing on managing testing infrastructure. Molecule is to provide an automated and repeatable environment to ensure the role works according to its specifications. Therefore, the test process destroys and re-creates the instances for every test.

Install [Molecule](https://molecule.readthedocs.io/en/latest/installation.html).

Check the complete [Molecule CLI](https://molecule.readthedocs.io/en/latest/usage.html).

Within the molecule/default folder, we find a number of files and directories:

- `molecule.yml` is the central configuration entry point for Molecule. With this file, you can configure each tool that Molecule will employ when testing your role.
- `converge.yml` is the playbook file that contains the call for your role. Molecule will invoke this playbook with ansible-playbook and run it against an instance created by the driver.
- `verify.yml` is the Ansible file used for testing as Ansible is the default Verifier. This allows you to write specific tests against the state of the container after your role has finished executing.

The full lifecycle sequence can be invoked with:

```bash
molecule test
```

By default, "molecule test" executes these steps in order:

1. Install required dependencies
2. Lint the project
3. Destroy existing instances
4. Run a syntax check
5. Create instances
6. Prepare instances (if required)
7. Converge instances by applying the role tasks
8. Check the role for idempotence
9. Verify the results using the defined verifier
10. Destroy the instances

You can change these steps by adding the “test_sequence” dictionary with the required steps to the Molecule configuration file.

To lint the entire project, run the following command from the project root:

```bash
molecule lint
```

The "converge" command applies the current version of the role to all the running container instances. Molecule “converge” does not restart the instances if they are already running. It tries to converge those instances by making their configuration match the desired state described by the role currently testing:

```bash
molecule converge
```

Using the verifier, you can define a playbook to execute checks and ensure the role produces the required results:

```bash
molecule verify
```

yamllint
----------------

The yamllint file checks YAML files for syntax and formatting issues.
