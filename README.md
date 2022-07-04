# Ansible — Dynamic DNS using Njalla

This playbook creates a bash script from the following fork. The script sends the public IP of an instance on Google Cloud to [Njalla](https://njal.la/). A [Cron](https://en.wikipedia.org/wiki/Cron) job runs the script once every hour.
* GitHub: [github.com/k3karthic/bash-updater/tree/gcloud](https://github.com/k3karthic/bash-updater/tree/gcloud)
* Codeberg: [codeberg.org/k3karthic/bash-updater/src/branch/gcloud](https://codeberg.org/k3karthic/bash-updater/src/branch/gcloud)

**Assumption:** Instance deployed using the Terraform script below,
- terraform__gcloud-instance
    - GitHub: [github.com/k3karthic/terraform__gcloud-instance](https://github.com/k3karthic/terraform__gcloud-instance)
    - Codeberg: [codeberg.org/k3karthic/terraform__gcloud-instance](https://codeberg.org/k3karthic/terraform__gcloud-instance)

## Code Mirrors

* GitHub: [github.com/k3karthic/ansible__gcloud-njalla-dns](https://github.com/k3karthic/ansible__gcloud-njalla-dns/)
* Codeberg: [codeberg.org/k3karthic/ansible__gcloud-njalla-dns](https://codeberg.org/k3karthic/ansible__gcloud-njalla-dns)

## Requirements

Install the following before running the playbook,
```
ansible-galaxy collection install community.general
pip install google-auth requests
ansible-galaxy collection install google.cloud
```

## Dynamic Inventory

The Google [Ansible Inventory Plugin](https://docs.ansible.com/ansible/latest/collections/google/cloud/gcp_compute_inventory.html) populates public FreeBSD instances.

All target FreeBSD instances must have the following,
* Label — `njalla_host: yes`
* Metadata entry — `njalla_domain: <domain>` and `njalla_domain_id: <record id>`

## Configuration

1. Create `inventory/google.gcp_compute.yml` based on `inventory/google.gcp_compute.yml.sample`
    1. Specify the project ID 
    1. Specify the zone where you have deployed your server on Google Cloud
    1. Configure the authentication
1. Set username and ssh authentication in `inventory/group_vars/
1. Set [API Token](https://njal.la/settings/api/) for Njalla in `inventory/group_vars/all.yml`. Use `inventory/group_vars/all.yml.sample` as a reference.

## Deployment

Run the playbook using the following command,
```
./bin/apply.sh
```

## Encryption

Encrypt sensitive files (SSH private keys) before saving them. `.gitignore` must contain the unencrypted file paths.

Use the following command to decrypt the files after cloning the repository,

```
$ ./bin/decrypt.sh
```

Use the following command after running terraform to update the encrypted files,

```
$ ./bin/encrypt.sh <gpg key id>
```
