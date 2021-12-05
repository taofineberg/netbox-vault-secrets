# Hashicorp Vault Plugin for Netbox

Provides convenient access to secrets stored in [Hashicorp Vault](https://www.vaultproject.io/) via the Netbox UI. You can attach secrets on a _Device_, _Virtual Machine_ or _Service_. The plugin is intended to serve as a possible replacement for the secrets functionality present in Netbox pre 3.0. The Netbox maintainers recommend replacing it with Vault.

The functionality is entirely client side. The plugin uses Javascript in the browser to access the Vault API directly. Your Netbox installation will never have access to the secrets or authentication credentials in Vault.

Secrets are stored at paths per a simple convention:
- `/device/{id}/{slug}` for Devices
- `/vm/{id}/{slug}` for Virtual Machines
- `/service/{id}/{slug}` for Services

## Installation

This plugin is not yet available as a PyPi package. Please see the [Releases](https://github.com/ffddorf/netbox-vault-secrets/releases) for downloads.

Please note that this plugin needs a run of  `python manage.py collectstatic` to work after being configured. For the official Docker image see the [official instructions](https://github.com/netbox-community/netbox-docker/wiki/Using-Netbox-Plugins#custom-docker-file).

## Setup

After installing the package, add the plugin to the Netbox configuration.

```py
PLUGINS = ["netbox_vault_secrets"]

PLUGINS_CONFIG = {
    "netbox_vault_secrets": {
        "api_url": "https://your-vault-deployment/", # can be relative
        "kv_mount_path": "/v1/secret",  # optional
        "secret_path_prefix": "/netbox",  # optional
    }
}
```

### Vault CORS settings

Note that if your Vault installation runs at a different _origin_ than Netbox, you need to [enable CORS](https://www.vaultproject.io/api/system/config-cors).

You can use this command (requires `sudo` privileges):

```sh
vault write /sys/config/cors enabled=true allowed_origins="*"
```

You can also set only the hostname of your Netbox deployment as an allowed origin.

Alternatively, proxy the Vault API on a subpath in your Netbox deployment, thereby moving it to the same origin, so no CORS setup is required.

## License

This code is licensed under the [2-clause BSD license](LICENSE.md).
