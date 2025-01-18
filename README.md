# Ansible Role: Microsoft Defender for Endpoint (MDATP)

This Ansible role installs and configures Microsoft Defender for Endpoint (MDATP) on Linux systems.

## Requirements

- Supported Linux distributions:
  - Red Hat Enterprise Linux (RHEL) 7.2 or higher
  - CentOS Linux 7.2 or higher
  - Ubuntu 16.04 LTS or higher
  - Debian 9 or higher
- Ansible 2.9 or higher
- Internet connectivity for package installation
- Valid Microsoft Defender for Endpoint license
- Onboarding package from Microsoft Defender Security Center

## Role Variables

All variables are defined in `defaults/main.yml` and can be overridden in your playbook. Here are the main variable groups:

### Installation Settings
```yaml
mdatp_package:
  name: "mdatp"
  state: "present"  # Options: present, latest, absent
  version: ""  # Leave empty for latest version
```

### Directory Configuration
```yaml
mdatp_paths:
  install_dir: /etc/opt/microsoft/mdatp
  config_dir: /etc/opt/microsoft/mdatp/conf
```

### Repository Configuration
```yaml
mdatp_repository:
  enabled: true
  state: "present"
  key_url: "https://packages.microsoft.com/keys/microsoft.asc"
```

### Update Settings
```yaml
mdatp_updates:
  enabled: true
  frequency: "daily"  # Options: daily, weekly
  automatic: true
```

### Security Settings
```yaml
mdatp_security:
  real_time_protection: true
  cloud_enabled: true
  sample_sharing: true
```

### Proxy Settings (Optional)
```yaml
mdatp_proxy:
  enabled: false
  server: ""
  port: ""
  user: ""
  password: ""
```

### Onboarding Configuration
```yaml
mdatp_onboarding:
  enabled: false  # Set to true when you want to onboard
  package_url: ""  # URL to your organization's onboarding package
  validate_cert: true  # Whether to validate SSL certificate when downloading
```

## Dependencies

None.

## Example Playbook

Here's a basic example:

```yaml
- hosts: servers
  vars:
    mdatp_package:
      state: present
    mdatp_security:
      real_time_protection: true
      cloud_enabled: true
    mdatp_updates:
      enabled: true
      automatic: true
  roles:
    - ansible-role-mdatp
```

Example with proxy and onboarding:

```yaml
- hosts: servers
  vars:
    mdatp_package:
      state: present
    mdatp_proxy:
      enabled: true
      server: "proxy.example.com"
      port: 8080
      user: "proxyuser"
      password: "proxypass"
    mdatp_onboarding:
      enabled: true
      package_url: "https://your-tenant.com/onboarding-package.zip"
  roles:
    - ansible-role-mdatp
```

## Tags

The role uses tags for selective execution:

- `installation`: Tasks related to package installation
- `configuration`: Tasks related to MDATP configuration
- `onboarding`: Tasks related to MDATP onboarding
- `security`: Tasks related to security settings
- `updates`: Tasks related to update settings
- `proxy`: Tasks related to proxy configuration

Example of using tags:
```bash
ansible-playbook playbook.yml --tags "installation,configuration"
```

## License

MIT

## Author Information

This role was created by Faris Alotaibi.

## Support

For issues with Microsoft Defender for Endpoint itself, please contact Microsoft Support.
For issues with this Ansible role, please open an issue on GitHub.
