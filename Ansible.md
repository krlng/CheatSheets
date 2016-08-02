# Ansible

```sh
playbook -i hosts playbook.yml -b -K # Run as root and get ask for sudo-pw
```

files in group_vars folder which are named after an inventory instance or group get included with them 



``` yaml
#Include custom hostfiles:
- include_vars: "{{ item }}"
  with_items:
  - "group_vars/config2.yml"
```

## Jinja

``` sh
{{ variable }}

{{ hostvars[h]['zookeeper_id'] }} # Access variables defined in the inventory
{{ hostvars[h]['ansible_fqdn'] }} # Access fully qualified domain name of an instance

# Apply calculations
{{ (ansible_memtotal_mb * yarn_memory_factor) | int }}

# Conditional parts
{{ if yarn_local_dirs is defined }} do something {{ endif }}

# Use loop and if statements
{{% for h in groups['zookeeper'] %}}{{ hostvars[h]['ansible_fqdn'] }}:2171{% if not loop.last %}{% endfor %}
```