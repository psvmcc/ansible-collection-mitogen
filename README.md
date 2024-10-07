# Collection for using Mitogen strategy

This collection allows to use [Mitogen](https://github.com/mitogen-hq/mitogen)
strategy without the need to specify absolute path to the strategy file.

It also performs live patching for oler versions of Mitogen (3.4), removing
Ansible version restrictions, making it possible to use Mitogen
on unsupported versions.

Tested versions of ansible-core:

* 2.14
* 2.15
* 2.16
* 2.17

When needed, it patches `ansible_mitogen` code and unpatches it
back right after module import, so the original
files are kept intact.

If no patching needed, it just passes mitogen content as it is.

## Install
To use this collection, you need to install Mitogen:

```bash
pip install mitogen==0.3.4
```

Then, you need to install this collection:

```bash
ansible-galaxy collection install serverscom.mitogen
```

## Usage

There are three ways to use this collection, and you need
to choose only one option.


### ansible.cfg
Add the following to your `ansible.cfg` file:
```
strategy = serverscom.mitogen.mitogen_linear
```

### Environment variable
You can set `ANSIBLE_STRATEGY` environment variable:

```
ANSIBLE_STRATEGY=serverscom.mitogen.mitogen_linear ansible-playbook ...
```

(This is my preferred way to use Mitogen).

### Strategy stanza

You can use a `strategy` stanza in a play:

```
- hosts: all
  strategy: serverscom.mitogen.mitogen_linear
  tasks:
   - debug:
```

# Dealing with mitogen bugs

Some code may not run under Mitogen. The easiest way to opt-out of Mitogen
for problematic plays is to use the `linear` strategy:

```
- hosts: all
  strategy: linear
  tasks:
    - debug:
```

# Acknowledgements
based on @ITD27M01 idea: https://github.com/mitogen-hq/mitogen/issues/961#issuecomment-1236291061

# Disclamer
Servers.com is not responsible for any problems that may arise
during the use of this collection.

Mitogen is a separate project (https://github.com/mitogen-hq/mitogen)
that is well-known for speeding up Ansible in exchange for multiple
stability issues. Servers.com does not provide support for
Mitogen-specific issues and may only address issues related
to imports and/or patching of Mitogen.

If you want to make Mitogen better, please help upstream.
