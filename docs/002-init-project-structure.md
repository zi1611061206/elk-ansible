Init basic project structure
```bash
mkdir -p ansible/{files,roles,inventories/{group_vars,host_vars}}
```

Create default playbook
```bash
touch ansible/site.yml
```

Create default inventory
```bash
touch ansible/inventories/hosts
```

Create ansible configuration
```bash
ansible-config init --disabled -t all > ansible/ansible.cfg
```