# Setting up Openshift on Binero

This guide describes how to install an Openshift cluster on Binero.

## Binero setup

1. Create a Binero account

2. Login to "Publikt moln"

3. Create an API user
    - Access & Säkerhet / API-användare / +
      - Namn: agronod
      - Lösenord: {password}

4. Open HORIZON

5. Login using API user (ensure user name is the full name generated when creating user, like 5002488_agronod)

<!-- 6. Generate key pair for cluster node SSH access
    - TODO

7. Lägg till nyckeln i HORIZON
    - Compute / Key Pairs / Import Public Key
      - Key Pair Name: agronod-key
      - Key Type: SSH Key
      - Public Key: kopiera in public key från tidigare steg -->

8. Download Openstack rc-file 
    - HORIZON / Account

9. Install openstack client

```bash
#mac
brew install openstackclient
```

10. Create floating IPs

```bash
#mac
source {path to rc-file}

openstack network list --long -c ID -c Name -c "Router Type"

export EXTERNAL_NETWORK=<external_network_id>

openstack floating ip create --description "API {cluster name}.agronod.com" $EXTERNAL_NETWORK

openstack floating ip create --description "Ingress {cluster name}.agronod.com" $EXTERNAL_NETWORK
```

11. Create security groups in HORIZON / Network / Security groups

    - bin-ssh
        - add rule 'ssh'

## DNS

- Add DNS records
    - API
      - type: A
      - ttl: 300
      - subdomain: api.{domain name}
      - data: {API floating-ip}
    - Ingress
      - type: A
      - ttl: 300
      - subdomain: *.apps.{domain name}
      - data: {Ingress floating-ip}

- Ensure DNS des not contain an "*.agronod.com" record

## Install Openshift

Reference [Openshift installation guide](https://docs.openshift.com/container-platform/4.11/installing/installing_openstack/installing-openstack-installer-custom.html)

1. Clone repo https://github.com/agronod/openshift-install

2. [Download installer](https://console.redhat.com/openshift/create)

    - Datacenter/Red Hat Openstack -> Installer provisioned
    - Move "openshift-install" into root of repo

3. Download installation pull secret from [redhat](https://console.redhat.com/openshift/install/pull-secret)

4. [Generate SSH key](https://docs.openshift.com/container-platform/4.11/installing/installing_openstack/installing-openstack-installer-custom.html#ssh-agent-using_installing-openstack-installer-custom)

```bash
#mac/linux
ssh-keygen -t ed25519 -N '' -f ~/.ssh/{cluster name}
```

5. Download "clouds.yaml" from HORIZON / API Access / Download Openstack RC file into repo root folder

6. Copy "install-config.yaml" into ```<installation directory>```

7. Update ```<installation directory>```/install-config.yaml

    - sshKey - ```cat ~/.ssh/{cluster name}.pub```
    - pullSecret - ```cat <path>/<pull_secret>```
    - ingressFloatingIP - from HORIZON / Network / Floating IPs
    - apiFloatingIP - from HORIZON / Network / Floating IPs
    - additionalSecurityGroupIDs - From HORIZON / Network / Security Groups

8. Generate manifests

```bash
./openshift-install --dir <installation_directory> create manifests
```

9. Update manifests

    - Add to manifests/cloud-provider-config.yaml
      - In section [LoadBalancer]
        - lb-provider = "amphora"

10. Create cluster

```bash
./openshift-install create cluster --dir <installation_directory> --log-level=debug
```
