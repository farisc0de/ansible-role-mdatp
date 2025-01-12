# Ansible Role: MDATP Agent Installation

This Ansible role installs and configures the Microsoft Defender for Endpoint (MDATP) agent on both Red Hat/CentOS-based and Ubuntu/Debian-based systems. It ensures the repository, keys, and required onboarding files are properly set up.

---

## Features

- Dynamically downloads and configures Microsoft Defender for Endpoint repositories.
- Supports both Red Hat/CentOS-based and Ubuntu/Debian-based systems.
- Downloads and extracts the onboarding package to the target directory.
- Ensures idempotency by checking for existing configurations and installations.

---

## Requirements

- Supported OS families:
  - **Debian/Ubuntu**: Tested on Ubuntu 20.04.
  - **Red Hat/CentOS**: Tested on CentOS 7.
- Ensure the following are installed on the control node:
  - `ansible` (version 2.9+ recommended)
- The role uses `ansible.builtin` modules, ensuring compatibility with modern Ansible installations.

---

## Role Variables

Below is a list of variables used in the role. These can be overridden in your playbooks or `group_vars`/`host_vars`.

### Required Variables

| Variable                 | Description                              | Example                                                       |
| ------------------------ | ---------------------------------------- | ------------------------------------------------------------- |
| `onboarding_package_url` | URL for the onboarding package zip file. | `https://example.com/WindowsDefenderATPOnboardingPackage.zip` |

### Optional Variables

| Variable                       | Default Value                                       | Description                                         |
| ------------------------------ | --------------------------------------------------- | --------------------------------------------------- |
| `mdatp_dir`                    | `/opt/mdatp`                                        | Directory where MDATP files are stored.             |
| `mdatp_onboard_file`           | `/opt/mdatp/mdatp_onboard.json`                     | Path to the extracted onboarding JSON file.         |
| `microsoft_key_url`            | `https://packages.microsoft.com/keys/microsoft.asc` | URL for the Microsoft GPG key.                      |
| `ubuntu_repo_template`         | Dynamic based on distribution version               | APT repository for Ubuntu/Debian systems.           |
| `redhat_repo_template.name`    | `microsoft-prod`                                    | Yum repository name for Red Hat/CentOS systems.     |
| `redhat_repo_template.baseurl` | Dynamic based on distribution version               | Yum repository base URL for Red Hat/CentOS systems. |

---

## Example Playbook

### Basic Usage

```yaml
- name: Install and configure MDATP
  hosts: all
  become: yes
  vars:
    onboarding_package_url: "https://example.com/WindowsDefenderATPOnboardingPackage.zip"
  roles:
    - role: mdatp
```

---

## Tags

The role supports the following tags for selective execution:

- `installation`: Run tasks related to repository and package installation.
- `configuration`: Run tasks for downloading and configuring the onboarding package.

---

## Dependencies

This role has no external dependencies but requires the following built-in Ansible modules:

- `ansible.builtin.apt`
- `ansible.builtin.yum_repository`
- `ansible.builtin.file`
- `ansible.builtin.unarchive`
- `ansible.builtin.get_url`

---

## Directory Structure

The role follows the standard Ansible Galaxy directory structure:

```plaintext
roles/
  mdatp/
    tasks/
      main.yml
      install.yml
      config.yml
    handlers/
      main.yml
    vars/
      main.yml
    defaults/
      main.yml
    files/
    meta/
      main.yml
```

---

## License

This project is licensed under the MIT License.

---

## Author Information

Created by Faris Alotaibi.

If you encounter any issues or have feature requests, feel free to open an issue on the project's repository.
