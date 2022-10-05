# Generate Loopia certificates

## Install certbot

```bash
# mac
brew install certbot
```

## Generate cert using Loopia plugin

Original guide can be found [here](https://github.com/runfalk/certbot-dns-loopia)

### Install certbot loopia plugn

```bash
sudo pip install certbot-dns-loopia
```

### Setup Loopia API user with permissions

- addZoneRecord
- getZoneRecords
- removeSubdomain
- removeZoneRecord

### Add credentials file

```ini
dns_loopia_user = user@loopiaapi
dns_loopia_password = passwordgoeshere
```

### Generate cert

```bash
sudo certbot certonly \
    --authenticator dns-loopia \
    --dns-loopia-propagation-seconds 90 \
    --dns-loopia-credentials credentials.ini \
    -d '*.agronod.com' \
    -d '*.apps.dev-ocp.agronod.com' \
    -d 'api.dev-ocp.agronod.com'
```

## Upload cert to Loopia

Find guide [here](https://support.loopia.se/wiki/lagga-upp-eget-ssl-certifikat/)