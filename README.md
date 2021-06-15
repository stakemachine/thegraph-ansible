# The Graph Indexer Ansible 
Collection of ansible playbooks and roles to operate The Graph Indexer infrastructure.  

## Demo
[![asciicast](https://asciinema.org/a/390813.svg)](https://asciinema.org/a/390813)

## Overview
This repo contains a number of roles and playbooks to run any kind of Indexer infrastructure deployments.  

Sample playbooks will show you how to use roles to run all components on a single node and provision infrastructure to multiple nodes.  


## Prerequisites
Make sure you have installed all of the following prerequisites on your machine:
* Python 3+
* Ansible ([Install instructions](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-and-upgrading-ansible-with-pip))
* Paramiko

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
Before using sample playbooks, install required roles:
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
##### Check 
If everything went ok, you should be able to ssh to your server and check the state of your setup.  
Check containers and their states:
```bash
docker ps
```
Check logs:
```bash
docker logs --tail 20 <container-name>
```
Also you can now connect to grafana at port 3000 and login/password from your vars file.

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
##### Check 
If everything went ok, you should be able to ssh to your servers and check the state of your containers.  
Check containers and their states:
```bash
docker ps
```
Check logs:
```bash
docker logs --tail 20 <container-name>
```
To get an access to grafana you need to connect to wireguard VPN that was created during first stage of deployment.


## Contributing

Contributions are always welcome!

## Authors

- [@stakemachine](https://github.com/stakemachine) ([twitter](https://twitter.com/stakemachine))

## License

[MIT](https://choosealicense.com/licenses/mit/)