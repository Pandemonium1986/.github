# Ansible convention

## Global

### Private variables

Any variable name contained in a playbook or role that is intended to be used only in a private context (not accessible by the user) **MUST** be preceded by a "\_" for example:

All variables contained in the `vars` folder:

```yaml
---
_packages:
  - python3-devel
  - sudo
_packages_pip:
  - python3-pip
_pip_executable: pip3
_pip_mandatory_packages:
  - setuptools
  - wheel
  - pipx
_python_executable: python3
```

All variables created for private purpose in `fact-***.yml` files:

```yaml
---
- name: Configure facts for any distribution
  ansible.builtin.set_fact:
    _global_distribution: "{{ ansible_distribution | lower }}"
    _global_os_family: "{{ ansible_os_family | lower }}"
    _global_distribution_version: "{{ ansible_distribution_version.split['.'](0) | lower }}"
```

### Public variables

Any variable name contained in a playbook or role intended for use by the end user **MUST NOT** be preceded by "\_" and **MUST** be preceded by the role name. The same applies to facts created from these variables. For example:

All variables contained in the `default` folder:

```yaml
---
init_groups:
  - group_name: pandama
    group_gid: 19860

init_users:
  - user_name: pandemonium
    user_comment: Pandemonium1986
    user_groups: ["pandemonium", "pandama"]
    user_home: /home/pandemonium
    user_password: $6$salt$IxDD3jeSOb5eB1CX5LBsqZFVkJdido3OUILO5Ifz5iwMuTS4XMS130MTSuDDl3aCI6WouIL9AjRbLCelDCy.g.
    user_shell: "/bin/zsh"
    user_uid: "1000"
```

All variables created for end user purpose in `fact-***.yml` files:

```yaml
- name: Generate pip_packages list
  ansible.builtin.set_fact:
    pip_packages: "{{ _pip_mandatory_packages + pip_packages }}"
```

## Playbook

TBD

## Collection

TBD

## Role

### Naming convention

Variables associated with a particular distribution **MUST** be located in a file named distro-[os_familly]-[os_version].yml

- distro-centos-7.yml
- distro-debian-12.yml
- distro-suse-tumbleweed.yml

Variables associated with a particular OS family **MUST** be found in a file named familly-[os_familly].yml

- family-debian.yml
- family-redhat.yml
- family-suse.yml

Variables associated with a particular distribution **MUST** be found in a file named setup-[os_familly].yml

- setup-debian.yml
- setup-redhat.yml
- setup-suse.yml

### Structure

Each role **MUST** contain an `assert.yml` file to test the validity of user-defined parameters.

```yaml
---
- name: Test pip_install_package_update
  ansible.builtin.assert:
    that:
      - pip_install_package_update is defined
      - pip_install_package_update is boolean
    quiet: true
    fail_msg: "Error pip_install_package_update MUST be boolean"
```

Each role **MUST** contain a `fact-global.yml` file for facts common to all roles and a `fact-role.yml` file for facts necessary for its use.
