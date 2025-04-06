# Secure Password Management for Elasticsearch Role

This document provides guidance on managing initial passwords for the Elasticsearch role to ensure security and customization during deployment.

## Importance of Secure Passwords

Default passwords in the Elasticsearch role are set to `change_me` to allow initial deployment without prior setup. However, leaving these default passwords in a production environment poses a severe security risk. You should change them immediately after deployment or before deployment through variables or Ansible Vault.

## How to Manage Passwords

### 1. Override Passwords via Variables

You can override default passwords by setting variables in `group_vars`, `host_vars`, or directly in your playbook. The main password variables include:

- `elasticsearch_bootstrap_password`: Password for the `elastic` user.
- `elasticsearch_builtin_users`: List of built-in users with their respective passwords (e.g., `beats_system`, `apm_system`, `kibana_system`, etc.).

Example in `group_vars/all.yml`:
```yaml
elasticsearch_bootstrap_password: "your_secure_password"
elasticsearch_builtin_users:
  - username: beats_system
    password: your_secure_beats_password
  - username: apm_system
    password: your_secure_apm_password
  - username: kibana_system
    password: your_secure_kibana_password
  - username: logstash_system
    password: your_secure_logstash_password
  - username: remote_monitoring_user
    password: your_secure_monitoring_password
```

### 2. Use Ansible Vault for Password Security

For enhanced security, you can store passwords in a file encrypted with Ansible Vault. This prevents storing passwords as plain text in source code.

- Create a Vault file, e.g., `group_vars/all/vault.yml`:
  ```bash
  ansible-vault create group_vars/all/vault.yml
  ```
- Add passwords to the Vault file:
  ```yaml
  elasticsearch_bootstrap_password: !vault |
    $ANSIBLE_VAULT;1.1;AES256
    ...
  ```
- Reference the Vault file in your playbook or variables.

When running the playbook, provide the Vault password or key file:
```bash
ansible-playbook site.yml --vault-password-file ~/.vault_pass.txt
```

### 3. Default Password Warning and Check

The Elasticsearch role includes a task to check if passwords are still set to the default `change_me`. If so, a warning message will be displayed, urging you to update the passwords for security. In production environments, you can set the variable `enforce_secure_passwords: true` to force deployment failure if default passwords are still in use.

## Conclusion

Managing secure passwords is critical to protecting your Elasticsearch cluster. Use the methods above to ensure passwords are properly set and do not remain at default values in critical deployments.
