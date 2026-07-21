# Hadoop + HBase cluster setup

## 1. Prepare SSH keys for Ansible
```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/hadoop_key -N ""
chmod 600 ~/.ssh/hadoop_key
```

Copy the public key to each host's `ubuntu` user (or your `ansible_user`):
```bash
ssh-copy-id -i ~/.ssh/hadoop_key.pub ubuntu@<host>
```

## 2. Update inventory with your host IPs
```bash
$EDITOR inventory/hosts.ini
```

## 3. Review cluster variables
```bash
$EDITOR group_vars/all.yml
```

## 4. Test connectivity
```bash
ansible all -i inventory/hosts.ini -m ping
```

## 5. Run the full setup
```bash
ansible-playbook -i inventory/hosts.ini site.yml -b
```

## 6. Verify HDFS
```bash
ansible namenode -i inventory/hosts.ini -b --become-user hadoop \
  -m shell -a "/opt/hadoop/bin/hdfs dfsadmin -report"
```

## 7. Verify HBase
```bash
ansible hmaster -i inventory/hosts.ini -b --become-user hadoop \
  -m shell -a "/opt/hbase/bin/hbase shell -n -e 'status'"
```

Web UIs (defaults):
- NameNode: `http://<nn-ip>:9870`
- HMaster: `http://<hm-ip>:16010`
- RegionServer: `http://<rs-ip>:16030`
