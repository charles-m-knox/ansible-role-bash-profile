# bash-profile

Ansible role that configures bash profiles the way I want.

It uses `/etc/profile.d` to curate a reasonable profile with aliases I use. It
also creates two directories:

- `~/.profile.d` - user-scoped shell profile scripts to `source` that are
  compatible with `sh` (and not `bash`)
- `~/.bash-profile.d` - user-scoped shell profile scripts to `source` that are
  compatible with `bash`

## Requirements

There are no requirements for this, it should work out of the box with Ansible.

## Role Variables

You must set `install_profile: true` in order for this role to make changes.

**Be careful, because this will delete most of the `.bashrc` configs from your
home directory.**

Otherwise, see `defaults/main.yml` for values that can be tweaked on a per-host
basis.

### Exported variables

Once run, this role also exports two variables by setting the following facts:

```conf
profile_user_profile_directory={{ ansible_env.HOME }}/.profile.d
profile_user_bash_profile_directory={{ ansible_env.HOME }}/.bashrc.d
```

You should use these directories in other places to configure your other
software the way you prefer.

## Dependencies

None at the moment.

## Usage

First, create a `requirements.yml` in your Ansible setup if you haven't already,
here's an example:

```yaml
---
collections:
  - name: community.general
  - name: ansible.posix
  - name: community.crypto

roles:
  - name: charles-m-knox.bash-profile
    src: https://github.com/charles-m-knox/ansible-role-bash-profile.git
    scm: git
    version: main
```

Next, install it:

```bash
# include the -f flag to forceably re-install and get the latest (equivalent to
# upgrading)
ansible-galaxy role install -f -r requirements.yml
```

Now, in your `site.yml` (or whatever your playbook is named), use the role -
note that root access is required for some of the steps:

```yaml
- name: bash-profile
  hosts: all
  roles:
    - charles-m-knox.bash-profile
  tags:
    - bash-profile
```

Some tasks may not be desirable, in which case you may want to step through
instead:

```bash
ansible-playbook site.yml --tags bash-profile --step
```

## License

MIT

## Author Information

<https://charlesmknox.com>
