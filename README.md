manage_packages
=========

![molecule workflow](https://github.com/straysheep-dev/ansible-role-manage_packages/actions/workflows/molecule.yml/badge.svg) ![ansible-lint workflow](https://github.com/straysheep-dev/ansible-role-manage_packages/actions/workflows/ansible-lint.yml/badge.svg)

This role is a mechanism to centralize basic package inventories. Predefined lists of packages to install or remove are here for specific system roles. You can extend and override everything using the variables provided.

| Package Manager | Status |
| --- | --- |
| apt | ✅ |
| dnf | ✅ |
| yum | ❌ |
| apk | ❌ |
| pkg | ❌ |
| pacman | ❌ |
| Snap | ✅ |
| Flatpak | ✅ |
| Winget | ❌ |

This role does not handle packages that require specific configuration (e.g. Sysmon, auditd) or packages that may need unique arguments to install (e.g. `pipx install --include-deps ansible`). Instead, use dedicated roles for such packages, with the goal being to define your complete builds in Ansible inventory vars. This role effectively creates the variable space in your inventory to store just what needs added or removed.

> [!NOTE]
> 1. To initialize submodules in this template, do: `git submodule update --init --recursive`
> 2. Replace all instances of `role_name` with the actual `role_name`, **EXCEPT FOR `role_name_check: 1` in `molecule.yml`**
> 3. Replace all instances of `ansible-role-template` with `ansible-role-<role_name>`
> 4. To update submodules, do: `git submodule update --remote --recursive`, see [straysheep.dev/resources/#git](https://straysheep.dev/resources/#git)

> [!IMPORTANT]
> **Git Submodules & CI**: The dockerfiles for molecule tests are maintained in a [monorepo](https://github.com/straysheep-dev/docker-configs) as submodules for maintainability / repeatability across all roles. Because of this, the CI workflow requires `actions/checkout` to have `submodules: 'recursive'`.

> [!TIP]
> For local development, don't forget to symlink your `<namespace>.<role_name>` to one of the paths Ansible expects roles to exist under. This is the alternative to using a relative file path in `molecule/converge.yml`.
>
> ```bash
> ln -s ~/src/ansible-role-role_name ~/.ansible/roles/<namespace>.role_name
> ```

Requirements
------------

- Ansible >= 2.10 (`meta/main.yml`).
- `community.general` collection for `snap` or `flatpak` install/remove lists.
- `winget` lists are accepted but not yet used (`tasks/winget.yml` is a stub).

Role Variables
--------------

All variables are self-documented and live in `defaults/main.yml`. They are safe to leave at their defaults (empty lists / `[]`) if unused.

Presets exist under `vars/main.yml`. Use `manage_packages_active_presets` / `manage_packages_presets_extra` to consume or customize them rather than editing that file directly.

Dependencies
------------

None.

Example Playbook
----------------

```yml
- name: Ubuntu base packages
  hosts: localhost
  connection: local
  become: true
  vars:
    manage_packages_active_presets:
      - ubuntu_base
    # Ad-hoc additions on top of the preset:
    manage_packages_apt_install:
      - nmap
  roles:
    - role: straysheep_dev.manage_packages
```

Run it against the current host:

```bash
ansible-playbook -i "localhost," -c local [--ask-become-pass] [-v] playbook.yml
```

License
-------

[MIT](./LICENSE)

Author Information
------------------

[straysheep-dev/ansible-configs](https://github.com/straysheep-dev/ansible-configs)

> [!NOTE]
> **AI-assisted Authorship**
>
> Drafts, examples, and research generated using [Claude](https://claude.com/product/overview), both in the web interface and via [Claude Code](https://code.claude.com/docs/en/overview) after ingesting the existing [ansible-configs](https://github.com/straysheep-dev/ansible-configs) codebase and reviewing the direction in a CLAUDE.md file.
>
> Assisted-by: Claude:claude-sonnet-5
>
