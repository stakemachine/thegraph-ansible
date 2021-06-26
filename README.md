# The Graph Indexer Ansible 
Collection of ansible playbooks and roles to operate The Graph Indexer infrastructure.  

## Demo
[![asciicast](https://asciinema.org/a/422354.svg)](https://asciinema.org/a/422354)

## Overview
This repo contains a number of roles and playbooks to run any kind of Indexer infrastructure deployments.  

Sample playbooks will show you how to use roles to run all components on a single node or provision infrastructure to multiple nodes.  


## Prerequisites
Make sure you have installed all of the following prerequisites on your machine:
* Python 3+
* Ansible ([Install instructions](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-and-upgrading-ansible-with-pip))
* Paramiko

Make sure that all servers have your ssh autorized_keys in place, so you won't be asked for a password during provisioning. If you don't have any keys, here is the easiest way to generate them and add them to your servers is to use [ssh-copy-id](https://www.ssh.com/academy/ssh/copy-id).

## Tested on
* Ubuntu 20.04

## Roles
* graph-node
* indexer-agent
* indexer-service
* postgres-docker
* prometheus
* grafana-docker
* vector-docker
* wireguard
* geerlingguy.postgresql
* geerlingguy.docker
* geerlingguy.pip
* geerlingguy.firewall


## Sample playbooks
Before using sample playbooks you need to install the required roles:
```bash
ansible-galaxy install -r requirements.yml
```
### Single Node
#### Configuration
It's necessary to adjust few variables before running an Indexer.
##### Variables
Copy an example
```bash
cp vars/single-node-testnet-no-vector.yml.example vars/single-node-testnet-no-vector.yml
```
Adjust variables in `vars/single-node-testnet-no-vector.yml`
##### Inventory
Copy an example
```bash
cp single-node.inventory.example single-node.inventory
```
Set an IP address of your server in `single-node.inventory`
##### Run playbook
```bash
ansible-playbook -i single-node.inventory single-node-testnet-no-vector.yml -u root -e @vars/single-node-testnet-no-vector.yml
```

### Multi-node
#### Configuration
Multi-node configuration is a bit more complicated because of the Wireguard VPN setup.  
But it's also possible to run without the wireguard but with a local network instead.
##### Variables
Copy an example
```bash
cp vars/multi-node-testnet-no-vector.yml.example vars/multi-node-testnet-no-vector.yml
```
Adjust variables in `vars/multi-node-testnet-no-vector.yml`
##### Inventory
Copy an example
```bash
cp multi-node.inventory.example multi-node.inventory
```
Set an IP address of your servers in `multi-node.inventory`.
Generate pairs of keys for each server and set them at the bottom of inventory file.  
To generate a pair of keys you can use these commands:
```bash
private_key=$(wg genkey)
public_key=$(echo $private_key | wg pubkey)
echo "private key: $private_key"
echo "public key: $public_key"
```
##### Run playbook
```bash
ansible-playbook -i multi-node.inventory multi-node-testnet-no-vector.yml -u root -e @vars/multi-node-testnet-no-vector.yml
```
Generate wireguard config to connect to network:
```bash
ansible-playbook -i multi-node.inventory wg_local.yml -u root
``` 

## Useful commands
### Check if nodes are accessible
```bash
ansible thegraph -m ping -i multi-node.inventory -u root
```
### Check containers and their states:
```bash
docker ps
```
### Check container logs:
```bash
docker logs --tail 20 <container-name>
```

## Contributing

Contributions are always welcome!

## Authors

- [@stakemachine](https://github.com/stakemachine) ([twitter](https://twitter.com/stakemachine))

## License

[MIT](https://choosealicense.com/licenses/mit/)