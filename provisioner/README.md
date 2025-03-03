# Provisioning
You will need to gather the following:
- An AAP subscription manifest. Place this file at: `provisioner/manifest.zip`
- An extra vars file containing the below information. Place this file at: `provisioner/extra_vars.yml`

A minimal extra vars file would look like:
```yaml
students: 5
dns_zone: sandbox0000.opentlc.com 
aap_download_token: long token acquired from https://access.redhat.com/management/api
aap_download_hash: SHA-256 checksum of current AAP 2.5 Containerized Setup Bundle for RHEL 9 acquired from https://access.redhat.com/downloads/content/480/ver=2.5/rhel---9/2.5/x86_64/product-software
```

You also need to provide overrides for `aws_region`, `aws_ami_rhel`, `aws_ami_nios`, and `aws_ami_gitea` if you want an AWS region that is not us-east-2.

This provisioner needs a couple collections that don't come with the RH-provided "supported" EE. To get collections set up in the right spot, `cd` into your repo clone and run the below command. Your `ansible.cfg` will need to have access to both `published` and `validated` repos in Automation Hub.

```
ansible-galaxy collection install -r collections/requirements.yml -p provisioner/collections
```

Ensure you have AWS credentials loaded in your environment:

```
export AWS_ACCESS_KEY_ID=
export AWS_SECRET_ACCESS_KEY=
```

Finally, run the provisioner:

```
ansible-navigator run provisioner/infoblox_workshop.yml -e @provisioner/extra_vars.yml
```
