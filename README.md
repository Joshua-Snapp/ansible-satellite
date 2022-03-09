# Ansible Playbooks and Roles
This playbook manages resources on Satellite servers.

**Table of Contents**

<!-- TOC -->

- [Set up environment to run playbooks](#set-up-environment-to-run-playbooks)
- [Run Playbook - satellite_provisioning.yml](#run-playbook---satellite_provisioningyml)
    - [LDAP Authentication](#ldap-authentication)
    - [Run all tasks, except the one that configures LDAP Auth](#run-all-tasks-except-the-one-that-configures-ldap-auth)
    - [Just run tasks to configure repos](#just-run-tasks-to-configure-repos)

<!-- /TOC -->

---

## Set up environment to run playbooks
The following section describes how to set up an environment to use the playbooks and roles in this repo.

The directions can be performed on any machine that has python installed. You'll need to be able to connect via ssh from the host you configure to the target hosts you intend to run the playbooks on.

1. Clone this repo.
1. Change directory into the repo.
1. Create a python virtual environment.
    - `python3 -m venv .venv`
1. Activate the virtual environment.
    - `source .venv/bin/activate`
1. Upgrade the setuptools and pip python modules in the virtual environment.
    - `python3 -m pip install --upgrade setuptools pip`
1. Install python modules defined in the `requirements.txt` file.
    - `pip install -r requirements.txt`

## Run Playbook - satellite_provisioning.yml

You'll need to export three environment variables that will define the URL of the specific Satellite server and the credentials to authenticate to the Satellite server.

Example:
```
export FOREMAN_SERVER_URL=https://some_satellite_hostname
export FOREMAN_USERNAME=some_satellite_admin_user
export FOREMAN_PASSWORD=<some_satellite_admin_user_password>
```

Check which hosts you're targeting before you run the playbook.

```
ansible-playbook satellite_provisioning.yml --list-hosts
```

List the playbook's tasks to see what it will perform.

```
ansible-playbook satellite_provisioning.yml --list-tasks
```

### LDAP Authentication
Run the following command to configure LDAP authentication:

```
ansible-playbook satellite_provisioning.yml --tags ldap_auth -e "ldap_auth_password=some_ldap_bind_account_password"
```

### Run all tasks, except the one that configures LDAP Auth
The following command will run all tasks, except for the one to configure LDAP Auth:

```
ansible-playbook satellite_provisioning.yml
```

### Just run tasks to configure repos
The following command will only run tasks to manage Red Hat repositories:

```
ansible-playbook satellite_provisioning.yml --tags repos
```
