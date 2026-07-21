# Hadoop + HBase Ansible setup

## Prerequisites
- Ubuntu hosts reachable over SSH
- Ansible installed on the control machine
- SSH key at `~/.ssh/hadoop_key` (or update inventory)

## Configure
1. Edit `inventory/hosts.ini` with real host IPs
2. Review `group_vars/all.yml` for versions, paths, and ports

## Run
```bash
ansible all -i inventory/hosts.ini -m ping
ansible-playbook -i inventory/hosts.ini site.yml -b
```

## Verify
```bash
ansible namenode -i inventory/hosts.ini -m shell \
  -a "/opt/hadoop/bin/hdfs dfsadmin -report" -b --become-user hadoop

ansible hmaster -i inventory/hosts.ini -m shell \
  -a "/opt/hbase/bin/hbase shell -n -e 'status'" -b --become-user hadoop
```
