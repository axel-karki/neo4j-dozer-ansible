## Neo4j + DozerDB Setup

### What is Neo4j?

**Neo4j** is a high-performance graph database that stores data as nodes, relationships, and properties. It is designed for querying and analyzing connected data at scale. Neo4j is widely used in domains such as recommendation engines, fraud detection, and network analysis.

### What is DozerDB?

**DozerDB** enhances the Neo4j **Community Edition** by adding **enterprise-grade features**—for free. It is fully open source under the GPL license. With DozerDB, you gain access to features normally only found in Neo4j Enterprise Edition, including enhanced security, backups, monitoring, and performance improvements.

---

## Installation Options

You have two playbooks depending on whether or not you want to use DozerDB.

---

### 1. Install Neo4j Without DozerDB

**Playbook**: `playbooks/main.yml`

* Installs the official Neo4j Community Edition using Docker.
* Configures port mappings, data directories, and credentials.

<img width="717" alt="Screenshot 2025-05-26 at 6 06 31 AM" src="https://github.com/user-attachments/assets/f74f3659-9ad3-42ec-93e5-6f49e284bf7d" />
<img width="717" alt="Screenshot 2025-05-26 at 6 07 12 AM" src="https://github.com/user-attachments/assets/150df79f-f035-43a9-b48a-3da6f9788425" />

---

### 2. Install Neo4j With DozerDB

**Playbook**: `playbooks/install_dozer_docker.yml`

* Pulls and runs the DozerDB-enhanced Neo4j Docker image.
* Provides Enterprise features on top of Neo4j Community Edition.
* Configuration includes data volumes, ports, and authentication.

```
root@debian:~# lsof -i :7687
COMMAND   PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
java    10447 neo4j  373u  IPv6  45311      0t0  TCP *:7687 (LISTEN)
```
<img width="718" alt="Screenshot 2025-05-26 at 6 07 45 AM" src="https://github.com/user-attachments/assets/868b2ce7-ce57-441c-9cc6-c0b460cf329b" />

---

## Configuration

To run the VM UI:
```bash
ssh -i <ssh_file_path> -L 7687:<vm_private_ip>:7687 -L 7474:<vm_private_ip>:7474 root@<server_public_ip>
```

To configure the Neo4j admin credentials, edit the file:

```bash
roles/neo4j/vars/main.yml
```

Set the variable:

```yaml
neo4j_password: your_secure_password
```

**Encrypt this file using Ansible Vault** to keep credentials secure:

```bash
ansible-vault encrypt roles/neo4j/vars/main.yml
```

To edit it later:

```bash
ansible-vault edit roles/neo4j/vars/main.yml
```

To run playbooks with the vault:

```bash
ansible-playbook playbooks/main.yml --ask-vault-pass
```

Or use a vault password file:

```bash
ansible-playbook playbooks/main.yml --vault-password-file ~/.vault_pass.txt
```

## Future Enhancements
* Backup for Neo4j
